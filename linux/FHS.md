# 🐧 Linux Filesystem Hierarchy (FHS)

> _"Everything is a file."_ — Linux Philosophy

The **Filesystem Hierarchy Standard (FHS)** defines the structure and purpose of directories in Linux.  
Understanding FHS helps you navigate, configure, and troubleshoot Linux systems effectively.  

---

## 📚 Contents

- [Overview](#overview)
- [Key Directories](#key-directories)

---

## Overview

- Linux uses a **single-rooted directory tree** starting at `/`.  
- Directories serve specific purposes, ensuring consistency across distributions.  
- Many modern distros have merged some directories (e.g., `/bin` → `/usr/bin`).  

---

## Key Directories

- bin : Essential executable binaries like `ls`, `cd`, etc.
- boot : Data required for booting the device
- dev : Location of special or device files (disks, USBs, etc.)
- etc : Configuration files. Host specific add-ons are added inside `/etc/opt/`
- home : Home directory for users
- lib : Essential shared libraries and kernel modules
- media : Mount point for removable media (floppy disks, cdroms or zip disks)
- mnt : Mount point for temporary mounted systems
- opt : Add-on application software packages
- proc : Virtual file system with kernel and process info
- root : Home directory for root user
- run : Runtime variable data
- sbin : System binaries -> symlinked with `/bin`
- srv : Data for services provided by the system
- tmp : Temporary files
- usr : Shared, read-only system-wide data and programs, including user-space applications, libraries, and documentation
- var : Variable data like logs, spool files, etc.
