# 📦 Modules and Packages

> _"Code is read much more often than it is written."_ — Guido van Rossum

Modules and packages allow you to organize your code into reusable, maintainable, and logically separated components.  
They help prevent code duplication, improve readability, and make large projects easier to manage.

## 📚 Contents

- [Modules](#modules)
- [Packages](#packages)
- [Import Mechanics](#import-mechanics)
- [Advanced Usage](#advanced-usage)
- [Adding a CLI](#adding-a-command-line-interface-cli)
- [Best Practices](#best-practices)

---

## Modules

A module is any Python (`.py`) file which defines functions, classes, variables or runnable code. Importing a module lets you reuse that code from other files.

**Common Import Formats:**

```python
import random
from datetime import date
from keras.layers import Sequential
import pandas as pd
from collections import defaultdict as dd
from math import *              # Not recommended for production
```

## Packages

A package is a directory which contains modules and subpackages within itself. Though modern packages do not need it, using `__init__.py` is explicit and safe. The structure of a package can be as follows:

```markdown
package/
    __init__.py
    module_1.py
    module_2.py
    subpackage/
        __init__.py
        submodule.py

```

## Import Mechanics

There are 2 types of imports - absolute (recommended) and relative.

**Absolute imports** uses the full path from the project root or Python's site packages

```python
from package.module import function

```

**Relative imports** use the dot notation with respect to the current modules relative position in the package hierarchy

```python
from .module import function                # Same directory
from ..subpackage.module import function    # One directory above

```

An issue that can come up during imports is **Circular Imports**. This happens when two modules import each other indefinitely, causing an import loop. Some ways to avoid it are:

- Refactor common code into a third module and import from there
- Use imports inside functions instead of top level imports

## Advanced Usage

### Controlled Exports

Exports can be controlled by defining imports as a list of strings in `__all__`. So, when a wildcard import is executed, only the functionalities specified which be imported. _NOTE: Unspecified functions can still be imported from the package directly using absolute imports_

```python
# module.py

__all__ = ["function_1", "Class1"]

def function_1():
    ...

def function_2():
    ...

class Class1:
    ...

class Class2:
    ...

```

### Defining Metadata

Metadata can be defined in special variables inside `__init__.py` or modules

```python
__author__ = "Soham Wadekar"
__version__ = "1.0.1"

```

### Namespace Packages

Traditionally, a package in Python is a directory containing an `__init__`.py file. This `__init__`.py can be empty or contain initialization code. Its presence tells Python, "Hey, this directory is a package."

But sometimes, you want to organize code across multiple directories or even multiple distributions (like plugins or extensions from different sources) but still have them appear as a single logical package. This is where Namespace Packages come in.

Starting Python 3.3, the import system automatically recognizes directories without `__init__`.py as namespace packages.

## Adding a Command-Line Interface (CLI)

To add a command-line interface to a package, follow these steps:

1. Create a separate file like `cli.py` which will handle command-line arguments.
2. Use the `argparse` module to parse command-line arguments.
3. Add an entry point in `setup.py` so that users can run the CLI after installing the package

```python
from setuptools import setup

setup(
    # ... other setup args ...
    entry_points={
        'console_scripts': [
            'cli-command = package.cli:main',
        ],
    },
)

```

### `argparse` module

I'll briefly go over the `argparse` module for better understanding. The simple workflow is like this:

> **Create a Parser → Add Arguments → Parse Arguments**

```python
import argparse

parser = argparse.ArgumentParser(description="Program description")

parser.add_arguments("arg1")            # Positional argument (mandatory)
parser.add_arguments("-a2")             # Shorthand optional argument
parser.add_arguments("--arg3")          # Optional argument

args = parser.parse_args()

print(args.arg1)

```

**Common `add_arguments` parameters**

| Parameter       | Description                                                      | Example                           |
|-----------------|------------------------------------------------------------------|---------------------------------|
| `name`          | The argument name (positional) or flag (optional)               | `"filename"`, `"-v"` or `"--verbose"` |
| `help`          | Description shown in help message                                | `"The input file"`               |
| `type`          | Data type for the argument (str, int, float, etc.)              | `type=int`                      |
| `default`       | Default value if the argument isn’t provided                     | `default=10`                    |
| `required`      | If an optional argument must be provided                         | `required=True`                 |
| `action`        | Special actions like `store_true` for flags                      | `action="store_true"`           |
| `choices`       | Restrict possible values                                         | `choices=["red", "green", "blue"]` |
| `nargs`         | Number of arguments (1, `?`, `*`, `+`, or integer)              | `nargs='+'` means 1 or more     |
| `metavar`       | Name for the argument in usage messages                          | `metavar="FILE"`                |

## Best Practices

### Organizing Project Structure

- Keep modules small and focused
- Use packages to group related modules
- Use descriptive names
- Avoid "God" modules - large modules with too many responsibilities

### Naming Conventions

- Use snake case (`snake_case`) for modules, packages, functions and variables
- Use upper snake case (`UPPER_SNAKE_CASE`) for constants
- Use Pascal case (`PascalCase`) for classes

### Documentation

- Use docstrings for modules, classes and functions
