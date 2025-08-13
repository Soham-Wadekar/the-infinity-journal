# 📦 RPM Packaging

> _"Bundle, distribute, and manage software with consistency."_ — RPM Philosophy

The **Red Hat Package Manager (RPM)** is a powerful packaging system used by many Linux distributions such as Red Hat Enterprise Linux (RHEL), Fedora, CentOS, and openSUSE.  
It allows developers and system administrators to create, install, update, and remove software in a standardized way, ensuring consistency and dependency tracking across systems.

## 📚 Contents

- [Purpose and Benefits](#purpose-and-benefits)
- [Common Commands](#common-commands)
- [Installation](#installation)
- [Workflow](#workflow)
- [Resources & Documentation](#resources--documentation)

---

## Purpose and Benefits

It allows you to package software (files, binaries, docs) into a `.rpm` file so that it can be easily installed, upgraded or removed with dependency tracking.

## Common Commands

### Querying and Verifying

- `rpm -q package-name`             Query a package
- `rpm -qa`                         List all installed packages
- `rpm -qi package-name`            Gives information about a package
- `rpm -ql package-name`            Lists files in a package
- `rpm -qf package-name`            Which packages owns a file
- `rpm -V package-name`             Verify package integrity

### Installing, Upgrading and Deleting

- `rpm -i package-name`             Query a Package
- `rpm -U package-name`             Query a Package
- `rpm -F package-name`             Query a Package
- `rpm --reinstall package-name`    Query a Package
- `rpm -e package-name`             Query a Package

For extra options, check this [documentation](https://man7.org/linux/man-pages/man8/rpm.8.html).

---

## Installation

Installing RPM is pretty easy.

On RedHat/Fedora/CentOS, run:

```bash
sudo dnf install rpm-build rpmdevtools
```

On openSUSE, run:

```bash
sudo zypper install rpm-build rpmdevtools
```

## Workflow

**Step 1:** Set up the RPM build environment using the following command:

```bash
rpmdev-setuptree

```

This will give the following structure:

```markdown
~/rpmbuild/
├── BUILD
├── RPMS
├── SOURCES
├── SPECS
└── SRPMS

```

**Step 2:** Create a tarball of the project folder and move it to `~/rpmbuild/SOURCES/`

```bash
tar czvf folder-name.tar.gz folder-name
mv folder-name.tar.gz ~/rpmbuild/SOURCES/

```

**Step 3:** Write an RPM spec file inside `~/rpmbuild/SPECS/folder-name.spec`. Details are mentioned in this [section](#writing-a-spec-file).

**Step 4:** Build the RPM

```bash
rpmbuild -ba ~/rpmbuild/SPECS/folder-name.spec

```

The RPM file will be stored inside `~/rpmbuild/RPMS/noarch/folder-name-1.0-1.noarch.rpm`. The name will be adjusted according to the specifications in the `.spec` file.

**Step 5:** You can then install the RPM using the install command.

---

### Writing a Spec File

An RPM `.spec` file is the blueprint that tells the RPM build system how to prepare, build, install, and package your software.
It is divided into two main parts:

- **Preamble** — Metadata and static information about the package

| Tag            | Description |
|----------------|-------------|
| **Name**       | The name of the package (should match `.spec` filename without `.spec`). |
| **Version**    | Upstream software version. |
| **Release**    | Package release number (increment when making changes without version bump). |
| **Summary**    | One-line short description of the package. |
| **License**    | License type (e.g., MIT, GPLv3). |
| **URL**        | Project or software homepage URL. |
| **Source0**    | Path/URL to the main source archive (e.g., `.tar.gz`). |
| **BuildArch**  | Target architecture (`noarch`, `x86_64`, `aarch64`, etc.). |
| **Requires**   | Runtime dependencies required for the package to work. |
| **BuildRequires** | Packages needed at build time (compilers, dev libs, etc.). |

- **Sections & Scriptlets** — Instructions for building, installing, and managing the package

| Tag            | Description |
|----------------|-------------|
| **%description** | Long description of the package, can span multiple lines. |
| **%prep**      | Preparation steps (usually unpack sources using `%setup`). |
| **%build**     | Compilation steps (e.g., `make`, `cmake`). |
| **%install**   | Installation steps (copy files into `%{buildroot}`). |
| **%files**     | List of files and directories to include in the package. |
| **%changelog** | History of package changes with maintainer name, email, and date. |
| **%pre**       | Script run before installation. |
| **%post**      | Script run after installation. |
| **%preun**     | Script run before uninstalling. |
| **%postun**    | Script run after uninstalling. |

---

## Resources & Documentation

Refer to this [documentation](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/packaging_and_distributing_software/introduction-to-rpm_packaging-and-distributing-software). I haven't yet delved into the advanced topics yet but plan to do so in the future.
