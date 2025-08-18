# 📦 Zypper

> _"One command to install, update, and manage it all."_ — Zypper Philosophy

Zypper is the command-line package manager for **openSUSE** and **SUSE Linux Enterprise (SLE)**.  
It provides a powerful interface to install, update, and manage software packages, as well as configure repositories and apply system patches.  

Built on top of the **libzypp** library, Zypper resolves dependencies automatically and integrates closely with YaST, making it a versatile and reliable tool for both desktops and servers.

---

## 📚 Contents

- [Repository Management](#repository-management)
- [Service Management](#service-management)
- [Software Management](#software-management)
- [Update Management](#update-management)
- [Querying](#querying)
- [Package Locks](#package-locks)
- [Locale Management](#locale-management)
- [Other Commands](#other-commands)

By streamlining software management, Zypper ensures your system stays secure, up-to-date, and consistent.  
Whether you’re maintaining a personal workstation or managing production servers, Zypper provides a fast, scriptable, and dependable way to handle software on SUSE-based systems.

---

## Repository Management

Handles software sources (repos) where packages are downloaded from.

- `repos, lr`             List all defined repositories.
- `addrepo, ar`           Add a new repository.
- `removerepo, rr`        Remove specified repository.
- `renamerepo, nr`        Rename specified repository.
- `modifyrepo, mr`        Modify specified repository.
- `refresh, ref`          Refresh all repositories.
- `clean, cc`             Clean local caches.

Repo files are stored in `/etc/zypp/repos.d/` as `.repo` files.

## Service Management

Manages higher-level services that provide or update repositories automatically.

- `services, ls`            List all defined services.
- `addservice, as`          Add a new service.
- `modifyservice, ms`       Modify specified service.
- `removeservice, rs`       Remove specified service.
- `refresh-services, refs`  Refresh all services.

## Software Management

Core package operations like installing, removing, or searching for software.

- `install, in`                   Install packages.
- `remove, rm`                    Remove packages.
- `removeptf, rmptf`              Remove (not only) PTFs.
- `verify, ve`                    Verify integrity of package dependencies.
- `source-install, si`            Install source packages and their build dependencies.
- `install-new-recommends, inr`   Install newly added packages recommended by installed packages.

## Update Management

Keeps the system secure and up-to-date through patches, updates, and upgrades.

- `update, up`            Update installed packages with newer versions.
- `list-updates, lu`      List available updates.
- `patch`                 Install needed patches.
- `list-patches, lp`      List available patches.
- `dist-upgrade, dup`     Perform a distribution upgrade.
- `patch-check, pchk`     Check for patches.

## Querying

- `search, se`            Search for packages matching a pattern.
- `info, if`              Show full information for specified packages.
- `patch-info`            Show full information for specified patches.
- `pattern-info`          Show full information for specified patterns.
- `product-info`          Show full information for specified products.
- `patches, pch`          List all available patches.
- `packages, pa`          List all available packages.
- `patterns, pt`          List all available patterns.
- `products, pd`          List all available products.
- `what-provides, wp`     List packages providing specified capability.

## Package Locks

Prevents specific packages from being updated, downgraded, or removed.

- `addlock, al`           Add a package lock.
- `removelock, rl`        Remove a package lock.
- `locks, ll`             List current package locks.
- `cleanlocks, cl`        Remove useless locks.

## Locale Management

Adds or manages language support and localized packages.

- `locales, lloc`         List requested locales (languages codes).
- `addlocale, aloc`       Add locale(s) to requested locales.
- `removelocale, rloc`    Remove locale(s) from requested locales.

## Other commands

Extra tools for system cleanup, verification, process handling, and general info.

- `help, ?`               Print zypper help
- `shell, sh`             Accept multiple commands at once.
- `versioncmp, vcmp`      Compare two version strings.
- `targetos, tos`         Print the target operating system ID string.
- `licenses`              Print report about licenses and EULAs of installed packages.
- `download`              Download rpms specified on the commandline to a local directory.
- `source-download`       Download source rpms for all installed packages to a local directory.
- `needs-rebooting`       Check if the reboot-needed flag was set.
- `ps`                    List running processes which might still use files and libraries deleted by recent upgrades.
- `purge-kernels`         Remove old kernels.
