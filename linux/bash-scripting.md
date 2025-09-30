# 🐚 Essential Bash Scripting Commands

> _"Scripting turns repetitive tasks into one-liners."_ — Anonymous

This guide covers the essential Bash scripting commands and concepts for automating tasks, managing files, and controlling system behavior efficiently.

---

## 📚 Contents

- [Basic Workflow](#basic-workflow)
- [Variables](#variables)
- [Conditionals](#conditionals)
- [Loops](#loops)
- [Functions](#functions)
- [Input/Output](#input--output)
- [Exit Codes](#exit-codes)
- [Process Control](#process-control)
- [Error Handling](#error-handling)

---

## Basic Workflow

To write a shell script, the file must begin with the "shebang" (#!) operator followed by the path to the interpreter

```bash
#! /bin/bash

Executable program here...
```

To run a bash script, the script must be made executable by changing its permissions. Add execution permissions by running the following command:

```bash
chmod u+x script.sh
```

To execute a shell script, simply run it on the command line as follows:

```bash
./script.sh
```

## Variables

Defined and used as follows:

```bash
name="Soham"

echo "Hello $name!"
```

Commands can be substituted inside variables

```bash
today=$(date)

echo "Today is $today"
```

_NOTE: No spaces are allowed around `=` while assigning variables._

## Conditionals

### If Statement

Syntax

```bash
if (condition); then
    # logic
elif (condition); then
    # logic
else
    # logic
fi
```

**Important Options**:
`-f file`: is a regular file?
`-d directory`: is a directory?
`-e file`: exists?
`-z string`: empty string?
`-n string`: non-empty string?
`==`/`!=`: string comparison
`-eq`, `-ne`, `-lt`, `-gt`: number comparison

### Switch Case

Syntax

```bash
case VARIABLE in
    pattern1)
        # logic
        ;;
    pattern2)
        # logic
        ;;
    *)
        # default logic
        ;;
esac
```

## Loops

### For Loop

Syntax

```bash
for VARIABLE in ITERATOR; do
    # logic
done

```

### While Loop

Syntax

```bash
while CONDITION; do
    # logic
done
```

## Functions

Syntax

```bash
func() {
    # example logic
    echo $1
}

func VARIABLE
```

`$1`, `$2` are arguments
`$0` is the script name
`$#` is the number of arguments
`$@` are all arguments

## Input / Output

**Redirection**:
`>`: Write to a file (overwrite)
`>>`: Append to a file
`<`: Read input from a file
`1>`: stdout
`2>`: stderr
`2>&1`: "send stderr to where stdout is going"

**Piping**: Connect output of one command to the input of another command (`|`)

## Exit Codes

Every command returns an exit status (`$?`)
`0` -> success, non-zero -> error

## Process Control

A process can be run in the background by add an `&` at the end. It can be brought back to the foreground using the `fg` command.

```bash
sleep 20 &
```

## Error Handling

Every command returns an exit code.
`1` → general error
`2` → misuse of shell builtins (or file not found in `ls`)
`126` → command invoked cannot execute
`127` → command not found
`128+n` → command terminated by signal `n`

If one command fails, the exit status helps you decide whether to continue or abort.

You can explicitly set your own exit code

Example Usage:

```bash
if [ $# -eq 0 ]; then
    echo "No arguments provided"
    exit 1   # failure
fi
```

By default, bash ignores non-zero exit codes (script keeps running).
`set -e` → exit immediately if any command fails.
