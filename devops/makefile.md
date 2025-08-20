# 🛠️ Makefile

> _"Don’t repeat yourself — automate it."_ — Make Philosophy  

A **Makefile** is a configuration file used by the **`make`** tool to automate repetitive tasks like compiling code, running tests, generating docs, or cleaning up temporary files.  
It defines **rules** (targets, dependencies, and commands) that describe how to build or update parts of a project.  

By codifying build steps in a Makefile, you can:  

- Save time (no need to type long commands repeatedly)  
- Ensure consistency (everyone builds the project the same way)  
- Avoid unnecessary work (only rebuilds files that changed)  
- Automate workflows (testing, deployment, CI/CD pipelines, etc.)  

Whether you’re building C programs, formatting Python code, or orchestrating DevOps tasks, Makefiles act as a **universal task runner** for your projects.  

---

## 📚 Contents

- [Make Tool Basics](#make-tool-basics)
- [Makefile Structure](#makefile-structure)
- [Variables](#variables)
- [Phony Targets](#phony-targets)
- [Pattern Rules](#pattern-rules)
- [Automatic Variables](#automatic-variables)
- [Conditionals](#conditionals)

---

## Make Tool Basics

The **`make` tool** is a build automation utility. It reads instructions from a file named **`Makefile`** (or `makefile`) and executes rules.  

- Default behavior: executes the **first target** in the file.  
- Usage:

```bash
  make          # runs the first target
  make clean    # runs the "clean" target
  make test     # runs the "test" target
```

## Makefile Structure

A Makefile should always be named as `Makefile` or `makefile`
A Makefile consists of rules, and each rule has three parts.

1. **Target:** The file or task you want to build
2. **Dependencies:** Files or targets that must exists or be up-to-date
3. **Recipe:** Shell commands to be run (must start with a TAB)

```makefile
target: dependencies
    recipe (command)

```

## Variables

Variables help avoid repetition and improves maintainability.

```makefile
PYTHON = python3

run:
    $(PYTHON) main.py
```

## Phony Targets

## Pattern Rules

## Automatic Variables

## Conditionals
