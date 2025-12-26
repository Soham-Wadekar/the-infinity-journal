# 🛠️ Makefile — Build Orchestration Language

A **Makefile** is a declarative build specification used by the `make` tool to **orchestrate tasks based on dependencies**.  
Instead of describing *how* to run steps imperatively, a Makefile defines *what depends on what*, and `make` figures out **what must run, in which order, and only when needed**.

Although originally designed for compiling C/C++ programs, Makefiles are now widely used for **build pipelines, automation, DevOps workflows, and task orchestration** across many languages and environments.

---

## 📚 Contents

- [Makefile Basics](#makefile-basics)
  - [Dependencies](#dependencies)
  - [Variables](#variables)
  - [Phony Targets](#phony-targets)

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

## Makefile Basics

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
