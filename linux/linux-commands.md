# 🖥️ Essential Linux Commands

> _"Mastering commands is mastering Linux."_ — Anonymous

This guide covers the essential Linux commands, their usage, and purpose.  
Knowing these commands helps you navigate, manage, and troubleshoot Linux systems efficiently.

---

## 📚 Contents

- [Navigation and File Management](#navigation-and-file-management)
- [Process Management Commands](#viewing-and-searching-files)
- [Network Commands](#process-and-system-management)
- [User and Permission Commands](#file-permissions-and-users)
- [System Information Commands](#networking-and-connectivity)
- [Package Management Commands](#compression-and-archiving)

---

## Navigation and File Management

| Command | Most Used Flags                                                  | Use Case / Description                                          |
| ------- | ---------------------------------------------------------------- | --------------------------------------------------------------- |
| `ls`    | `-l` (long), `-a` (all), `-h` (human-readable), `-R` (recursive) | List files and directories with details, including hidden files |
| `cd`    | N/A                                                              | Change directories (`cd /home`, `cd ~`, `cd -`)                 |
| `pwd`   | N/A                                                              | Print current working directory                                 |
| `mkdir` | `-p`                                                             | Create directories, including nested directories                |
| `rmdir` | `-p`                                                             | Remove empty directories recursively                            |
| `cp`    | `-r` (recursive), `-i` (interactive), `-v` (verbose)             | Copy files and directories                                      |
| `mv`    | `-i`, `-v`                                                       | Move or rename files and directories                            |
| `rm`    | `-r` (recursive), `-f` (force), `-i` (interactive)               | Remove files and directories                                    |
| `tree`  | `-L N` (levels), `-a`                                            | Show directory tree structure                                   |

## Viewing and Searching Files

| Command | Most Used Flags                                                         | Use Case / Description                          |
| ------- | ----------------------------------------------------------------------- | ----------------------------------------------- |
| `cat`   | `-n`                                                                    | Display file content with line numbers          |
| `less`  | `-N`                                                                    | Scroll through large files, show line numbers   |
| `head`  | `-n N`                                                                  | Show first N lines of a file                    |
| `tail`  | `-n N`, `-f`                                                            | Show last N lines; `-f` to monitor live updates |
| `grep`  | `-i` (ignore case), `-r` (recursive), `-n` (line number), `-v` (invert) | Search text in files                            |
| `find`  | `-name`, `-type`, `-size`                                               | Search for files by name, type, or size         |
| `wc`    | `-l` (lines), `-w` (words), `-c` (bytes)                                | Count lines, words, or characters in a file     |
| `cut`   | `-d` (delimiter), `-f` (field)                                          | Extract specific columns from text              |
| `sort`  | `-n` (numeric), `-r` (reverse), `-u` (unique)                           | Sort lines in a file                            |
| `uniq`  | `-c` (count), `-d` (duplicates)                                         | Filter duplicate lines, often used with `sort`  |

## Process and System Management

| Command   | Most Used Flags                  | Use Case / Description                           |
| --------- | -------------------------------- | ------------------------------------------------ |
| `ps`      | `aux`, `-ef`                     | List running processes with details              |
| `top`     | `-d N` (refresh delay), `-p PID` | Monitor system processes and resource usage live |
| `htop`    | Interactive                      | Enhanced top with sorting, killing, filtering    |
| `jobs`    | `-l`                             | Show background jobs and their PIDs              |
| `kill`    | `-9` (SIGKILL), `-15` (SIGTERM)  | Terminate a process by PID                       |
| `killall` | `-9`                             | Kill all processes by name                       |
| `uptime`  | N/A                              | Show system load averages                        |
| `free`    | `-h`                             | Display memory usage in human-readable format    |
| `df`      | `-h`, `-T`                       | Show disk usage and filesystem types             |
| `du`      | `-h`, `-s`                       | Show directory/file sizes                        |

## File Permissions and Users

| Command  | Most Used Flags  | Use Case / Description                     |
| -------- | ---------------- | ------------------------------------------ |
| `chmod`  | `u/g/o`, `+/-/=` | Change file permissions (`chmod u+x file`) |
| `chown`  | `user:group`     | Change file ownership                      |
| `chgrp`  | `group`          | Change group ownership                     |
| `ls -l`  | N/A              | View file permissions and ownership        |
| `id`     | N/A              | Display current user UID, GID, and groups  |
| `groups` | N/A              | Show all groups for current user           |
| `whoami` | N/A              | Print current logged-in user               |
| `sudo`   | N/A              | Execute commands as root                   |

## Networking and Connectivity

| Command                | Most Used Flags           | Use Case / Description               |
| ---------------------- | ------------------------- | ------------------------------------ |
| `ping`                 | `-c N`                    | Test network connectivity to host    |
| `ifconfig` / `ip addr` | `ip a`, `ip link`         | View or configure network interfaces |
| `netstat`              | `-tuln`, `-p`             | Show listening ports and connections |
| `ss`                   | `-tuln`                   | Modern alternative to `netstat`      |
| `curl`                 | `-I` (headers), `-X` (method), `-v` (verbose), `-d` (data) | Make HTTP requests, test APIs        |
| `wget`                 | `-O filename`             | Download files from web              |

## Compression and Archiving

| Command     | Most Used Flags                                              | Use Case / Description                 |
| ----------- | ------------------------------------------------------------ | -------------------------------------- |
| `tar`       | `-c` (create), `-x` (extract), `-z` (gzip), `-j` (bzip2), `-v` (verbose), `-f` (filename) | Archive and extract files              |
| `gzip`      | `-d` (decompress)                                            | Compress / decompress single files     |
| `zip/unzip` | `-r` (recursive)                                             | Compress/Extract zip files recursively |

## Miscellaneous Utilities

| Command   | Most Used Flags      | Use Case / Description                  |
| --------- | -------------------- | --------------------------------------- |
| `date`    | `+%Y-%m-%d %H:%M:%S` | Show current date/time or format output |
| `echo`    | `-e`                 | Print text; interpret escape sequences  |
| `sleep`   | N/A                  | Pause script for N seconds              |
| `history` | `!N`, `-c`           | View command history, rerun commands    |
| `alias`   | N/A                  | Create shortcuts for commands           |
| `clear`   | N/A                  | Clear terminal screen                   |
