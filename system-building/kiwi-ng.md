# 🖼️ KIWI NG

> _"Build custom Linux appliances and system images with precision."_ — KIWI NG Philosophy

KIWI NG (Next Generation) is a powerful command-line tool for creating Linux system appliances and images.  
It helps you build ready-to-deploy OS images tailored to specific use cases, whether virtual machines, live ISOs, or cloud images.

## 📚 Contents

- [What is KIWI NG?](#what-is-kiwi-ng)
- [Key Features](#key-features)
- [Installation](#installation)
- [Test Image Build](#test-image-build)
- [Basic Workflow](#basic-workflow)
- [Useful Commands](#useful-commands)
- [Images](#images)
- [Resources & Documentation](#resources--documentation)

KIWI NG streamlines the complex process of assembling OS images by automating filesystem creation, package installation, and image packaging.  
It is especially popular in the SUSE/openSUSE ecosystem but adaptable for other Linux distributions as well.

---

## What is KIWI NG?

KIWI NG is a Linux appliance builder tool that creates bootable images based on declarative XML appliance descriptions.  
It supports multiple image types such as ISO, raw disk images, and cloud images.

---

## Key Features

- Flexible appliance description format
- Supports multiple image output formats
- Integration with RPM-based and other package managers
- Automation of filesystem and image creation
- Suitable for building virtual machine images, live CDs, embedded systems, and more

---

## Installation

I tried multiple ways to install KIWI NG on my Ubuntu machine, but faced a lot of issues. KIWI NG is available in the package manager of openSUSE. So, I decided to create a container running openSUSE inside, with Docker. The installation is with respect to it:

**Step 1:** Copy the following Dockerfile. (It is custom made to install KIWI NG). Essentially, it uses openSUSE as a base image, installs the packages required to test KIWI NG, and clones the KIWI NG Git repository inside it. The shell starts by default, once the container runs.

```Dockerfile
FROM opensuse/leap:15.5

# Install required packages
RUN zypper refresh && \
    zypper install -y \
        python3-kiwi \
        git \
        qemu \
        qemu-tools \
        gptfdisk \
        kpartx \
        dosfstools \
        btrfsprogs \
        squashfs \
        xorriso && \
    zypper clean --all

# Set working directory
WORKDIR /workspace

# Clone KIWI repo into /workspace
RUN git clone https://github.com/OSInside/kiwi

# Start with bash shell
CMD ["/bin/bash"]

```

**Step 2:** Build the Docker image by running the following command:

```bash
docker build -t kiwi-ng:latest .
```

**Step 3:** Run a Docker container using the image in `privileged` mode. You will be taken inside the openSUSE shell as `root`.

```bash
docker run -it --privileged kiwi-ng:latest
```

**Step 4:** Once inside the container, verify the KIWI installation using the following command:

```bash
kiwi-ng --version
```

## Test Image Build

Run this command:

```bash
kiwi-ng system build \                                  
    --description kiwi/build-tests/x86/leap/test-image-disk \           # Build from this description
    --set-repo https://download.opensuse.org/distribution/leap/15.6/repo/oss \      # Overrides the default repo
    --target-dir ../tmp/myimage     # Stores the images in this folder

```

Edit the `config.sh` file inside `kiwi/build-tests/x86/leap/test-image-disk`, by adding the following line at the end. This will create a username and password for you to login to the QEMU VM.

```bash
echo "root:root" | chpasswd         # uname:passwd format 

```

It might take some time to create the images. Once done, you can run your image (boot the OS). Run it using a virtual disk like QEMU.

```bash
qemu-system-x86_64 -boot c \
  -drive file=../tmp/myimage/kiwi-test-image-disk.x86_64-1.15.6.raw,format=raw,if=virtio \
  -m 4096 -serial stdio

```

**Explanation:**
This is really confusing for me too, so thought I should elaborate, for my sake more than yours.

- `qemu-system-x86_64`: Runs the QEMU emulator for 64-bit x86 CPUs
- `-boot c`: Tells QEMU which device to boot from. `C` refers the first hard disk.
- `-drive`: Adds a virtual drive to the VM
- `file=...`: Path to the disk file created by KIWI(`.raw` format)
- `format=raw`: Tells QEMU the image format is raw
- `if=virtio`: Use VirtIO interface (high-performanced paravirtualized disk) for the disk
- `-m 4096`: Allocates 4096MB (4GB) of RAM for the VM
- `-serial stdio`: Redirects the guest’s serial port to your host’s terminal stdin/stdout

Confused at what I did just now? Don't worry, the mindfuck is real. Basically, what I did was:

1. Ran Ubuntu on host machine
2. Started a Docker container with openSUSE Leap as its OS
3. Built a raw image using KIWI
4. Created a QEMU VM inside the container using the image
5. Booted another openSUSE Leap OS inside the QEMU VM

Here's a visualization for better understanding:

```markdown
Host Machine (Ubuntu)
└── Docker Container (openSUSE Leap)
    ├── KIWI NG (builds `.raw` image)
    └── QEMU Virtual Machine
        └── KIWI-built OS (from `.raw` disk image)
```

---

## Basic Workflow

When KIWI builds a Linux image, it follows these steps:

1. First, it reads the image description which is specified in a `config.xml` or `.kiwi` file. This file tells KIWI what kind of image to make (e.g., ISO, virtual disk, etc.), which packages to include, and other settings.
2. Then, it enters the **Prepare Phase**, where it creates a root tree containing all the future system files. It installs all packages from the repository that you defined.
3. Then, it enters the **Create Phase**, where it converts the root tree into the final image format (ISO, VMDK, container, etc.)

Your image is then ready to be booted!

---

## Useful Commands

The basic syntax of any KIWI NG command is `kiwi-ng [--global-options] service <command> [args]`. For more details, see [global options](https://osinside.github.io/kiwi/commands/kiwi.html).

1. `kiwi-ng result list`: Lists build results from a previous `build` or `create` command. [Options](https://osinside.github.io/kiwi/commands/result_list.html#options) are listed here.
2. `kiwi-ng result bundle`: Bundles all related build files in a single bundle file for easy transfer, storing or sharing. [Options](https://osinside.github.io/kiwi/commands/result_bundle.html#options) are listed here.
3. `kiwi-ng system build`: Builds an image from a description. It runs both `prepare` and `create` commands. [Options](https://osinside.github.io/kiwi/commands/system_build.html#options) are listed here.
4. `kiwi-ng system prepare`: Sets up a new image root directory from the specified XML description. [Options](https://osinside.github.io/kiwi/commands/system_prepare.html#options) are listed here.
5. `kiwi-ng system create`: Creates an image from the previously specified image root directory. [Options](https://osinside.github.io/kiwi/commands/system_create.html#options) are listed here.
6. `kiwi-ng system update`: Updates a previously prepared image root tree. [Options](https://osinside.github.io/kiwi/commands/system_update.html#options) are listed here.
7. `kiwi-ng image resize`: Allows resizing for disk-based images. [Options](https://osinside.github.io/kiwi/commands/image_resize.html#options) are listed here.
8. `kiwi-ng image info`: Provides information about a specified image description. [Options](https://osinside.github.io/kiwi/commands/image_info.html#options) are listed here.

---

## Images

### Image Description Elements

This section includes the required and optional elements that make up a valid image description.

| Element        | Purpose                                                                                          | Required? | Repetition                  |
|----------------|--------------------------------------------------------------------------------------------------|-----------|-----------------------------|
| `<image>`      | Root element containing the entire image description.                                            | Yes       | Only once                   |
| `<description>`| Identity of the image: author, contact, license, purpose/specification.                          | Yes       | Only once                   |
| `<preferences>`| Configuration of the image: type, version, layout, partitioning.                                 | Yes       | Can repeat for different profiles/architectures |
| `<repository>` | Defines software sources from which packages are installed.                                      | Yes       | Can repeat                   |
| `<packages>`   | Lists software to include: packages, collections, archives, products.                            | Yes       | Can repeat                   |
| `<users>`      | Predefines system user accounts to be included in the image.                                     | No        | Can repeat                   |
| `<profiles>`   | Defines selectable build profiles/namespaces in the image description.                           | No        | Only once                    |
| `<include>`    | Imports XML content from an external file into the description (single-level include only).      | No        | Can repeat                   |

### Image Types

| Image Type           | Purpose                                                                                           |
|----------------------|---------------------------------------------------------------------------------------------------|
| **ISO Hybrid Live Image**          | Portable bootable system from CD/DVD/USB for testing or debugging.                                |
| **Virtual Disk Image**             | Disk image for use in cloud environments (e.g., EC2, GCE, Azure).                                 |
| **OEM Expandable Disk Image**      | Disk image that auto-resizes to deployment hardware; supports installer media outputs.            |
| **Docker Container Image**         | Tarball loadable via `docker load`, ready for container use.                                       |
| **WSL Container Image**            | Image suitable for Windows Subsystem for Linux (WSL) environments.                                |
| **KIS Root File System Image**     | Root filesystem image bundled with kernel and initrd—increasable for custom deployment use cases. |
| **AWS Nitro Enclave Image**        | EFI-format initrd image for AWS Nitro Enclave or QEMU testing.                                 |

Once KIWI creates or builds an image, it generates several output files.

1. **Main Image Binary:** It is named as `<image-name>.<architecture>-<version>.<extension>`.
2. **Packages List:** Generates a file which has a CSV-like dump of all the packages included in the image. It is named as `<image-name>.<architecture>-<version>.packages`
3. **Verification File:** Generated to test the integrity of the package before it is used. It is named as `<image-name>.<architecture>-<version>.verified`

---

## Resources & Documentation

This is the craziest technology I have ever worked on. It has so little documentation on the Internet that learning it gives you a headache. I found no YouTube videos or other resources on usual websites. My only source was the [official documentation](https://osinside.github.io/kiwi/index.html), and some help from ChatGPT for explaining the entire ordeal.

> **EDIT:** I gave some time, and read the documentation and it doesn't feel that difficult. Essentially, KIWI NG is a tool which builds an image (a fully-configured Linux system) from an XML file. These images can be used to run virtual machines, create bootable USB drives, containers and more.
