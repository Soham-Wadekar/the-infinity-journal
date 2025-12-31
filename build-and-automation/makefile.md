# 🛠️ Makefile — Build Orchestration Language

A **Makefile** is a declarative build specification used by the `make` tool to **orchestrate tasks based on dependencies**.  
Instead of describing *how* to run steps imperatively, a Makefile defines *what depends on what*, and `make` figures out **what must run, in which order, and only when needed**.

Although originally designed for compiling C/C++ programs, Makefiles are now widely used for **build pipelines, automation, DevOps workflows, and task orchestration** across many languages and environments.

---

## 📚 Contents

- [Makefile Basics](#-makefile-basics)
  - [Dependencies](#dependencies)
  - [Variables](#variables)
  - [Phony Targets](#phony-targets)
- [Makefile Intermediates](#-makefile-intermediates)
  - [Automatic Variables](#automatic-variables)
  - [Pattern Rules](#pattern-rules)
  - [Makefile Verbosity](#makefile-verbosity)
  - [Chaining Commands](#chaining-commands)
  - [Modularization](#modularization)
  - [Variables from the Command Line](#variables-from-the-command-line)

---

## ✨ Why Makefile?

Makefile shine when you need:

- Deterministic task execution
- Dependency-aware workflows
- Minimal re-execution of commands
- A lightweight alternative to long shell scripts
- A tool that works everywhere (Linux, CI, containers)

They act as a **single entry point** for project operations such as build, test, lint, package, and deploy.

---

## 🟢 Makefile Basics

`make` is a build orchestration tool that decides what commands to run, in what order, and only when needed, based on file dependencies.

Simple syntax of a Makefile is:

```Makefile
TARGET: DEPENDENCIES
<TAB>COMMAND
```

### Dependencies

Essentially, `make` creates a dependency graph (target depends on what), checks whether any dependency has changed, and builds the target only then (runs the underlying commands)

```Makefile
build: clean link compile
    echo "Build Done!"

clean:
    echo "Cleaning..."

link:
    echo "Linking..."

compile:
    echo "Compiling..."
```

When `make build` is run, it will first run `clean`, then `link` and `compile` and only then will run `build`. This means, other targets must run in that specific order before `build` is run.

### Variables

|Symbol|Meaning|
|------|-------|
|`=`|Assigns lazily. Evaluated during runtime|
|`:=`|Evaluated immediately|
|`?=`|Assigns only if not set|
|`+=`|Appends to an existing variable|

Examples:

```Makefile
NAME = John                 # Sets John when target is called

GREETING := Hello $(NAME)   # Evaluates to "Hello John". Won't change if NAME is changed later

PLACE ?= Earth              # If PLACE is not defined, it defaults to "Earth"

NAME += Doe                 # NAME becomes "John Doe"
```

Shell commands can also be assigned through variables as follows:

```Makefile
DATE = $(shell date +%F)
```

### Phony targets

By default, make assumes that every target is a file which needs to be built.

```Makefile
test:
    echo Testing...
```

When we do `make test`, if a folder or file named test is present, it will try to build the file instead of echoing "Testing...". To avoid this, added `.PHONY` target, at the top of the Makefile

```Makefile
.PHONY: test

test:
    echo Testing...
```

`.PHONY` tell `make` that the target is an action NOT a file.
> Use .PHONY for any target that represents an **action**, not a file.

## 🟡 Makefile Intermediates

### Automatic Variables

Automatic variables are not set by the user but rather expand when the target is called. They remove repetition and make rules reusable.

|Variable|Meaning|
|----------|---------|
|`$@`|Target name|
|`$<`|First dependency|
|`$^`|All dependencies|
|`$*`|Stem (pattern match)|

Example:

```Makefile
output.txt: input.txt
    cp $< $@
```

is equivalent to

```bash
cp input.txt output.txt
```

### Pattern Rules

Sometimes we need to create same rules for multiple different files. In such scenarios, instead of separate targets, we can write a single pattern rule using `%`.

```Makefile
%.out: %.in
    cp $< $@
```

This means, that any file with the extension `.out` can be called as a target, as long as an equivalent `.in` file is present.

If a file needs to be created inside a directory, but the directory might not be present, a small fallback can be added using `|`

```Makefile
build/%.out: src/%.in | build
    cp $< $@

build:
    mkdir -p build
```

Creates `build` directory if it doesn't exist, so that the target doesn't fail.

### Makefile Verbosity

The default output of a Makefile is verbose. That means, Makefile will always print the command while running. There are multiple methods to avoid that.

#### Silent command

Prefix the command with a `@`

```Makefile
target:
    @echo
```

#### Silent entire Makefile

Add this to the top

```Makefile
MAKEFLAGS += --silent
```

#### Toggle verbosity

```Makefile
ifeq ($(V),1)
    Q :=
else
    Q := @
endif

build:
    $(Q)echo "Building..."
```

```bash
make        # silent
make V=1    # verbose
```

### Chaining Commands

Make runs every line in a separate shell, so listing down commands isn't feasible. Commands can be chained using `&&` or `; \`

```Makefile
build:
    cd dir && build_something

test:
    cd dir; \
    test_something; \
    echo Done!
```

> Hint: Avoid running many commands in a single target. Keep them inside scripts instead of a Make rule, as Make is a build orchestration tool and not a scripting language.

### Modularization

Make parses first, executes later. Different Makefiles can be added using the keyword `include`. It basically tells Make, *"“Before you finish parsing, paste the contents of the included Makefile here.”"*. It basically creates a huge Makefile (there is no boundary between files). It is textual inclusion, not function calls, not imports.

Syntax:

```Makefile
include config.mk
```

Variables and targets are defined globally, but can be overridden if the variable is redefined *after* the import.

If a file doesn't exist, we can include it optionally. It is also usually paired with [conditional imports]().

```Makefile
-include config.mk
sinclude config.mk
```
<!-- Add link here when defined -->

### Variables from the Command Line

Variables can be passed through the command line. They can override the variables defined in the Makefile.

```Makefile
IMAGE = default.img

build:
    echo "Building $(IMAGE)"
```

Override `IMAGE` as follows:

```bash
make IMAGE=test.img
```
