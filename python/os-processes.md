# 🖥️ OS and Processes in Python

> _"Don't communicate by sharing memory; share memory by communicating."_ — Go proverb

Working with the operating system and processes allows your Python programs to interact directly with the environment they run in.  
From reading system information to launching and controlling other programs, these capabilities open up powerful automation, orchestration, and integration possibilities.

## 📚 Contents

- [OS Module Basics](#os-module-basics)
- [Running External Commands](#running-external-commands)
- [Interacting with Child Processes](#interacting-with-child-processes)

By understanding how to execute files, read/write to standard I/O streams, and manage child processes, you can build scripts that not only respond to their environment but also control it — making your Python programs true citizens of the operating system.

---

## OS Module Basics

> ⚠️ UNDER CONSTRUCTION: This part will be added here later!

---

## Running External Commands

Using the `subprocess` module, we can run other programs or scripts.

```python
import subprocess

result = subprocess.run(
    ["command", "as", "a", "list", "of", "words"],
    capture_output=bool,        # Captures the value in Python
    text=bool                   # Returns readable text 
)

result.stdout                   # Output
result.stderr                   # Error

```

---

## Interacting with Child Processes

Sometimes you need to start a program and communicate with it while it’s running. We can do that using `.Popen()` method.
- `stdin`, `stdout` and `stderr` provide input, output and error communication in the pipeline
- Input can be sent using `.communicate()` method

---