# 🖼️ KIWI NG

> _"Build custom Linux appliances and system images with precision."_ — KIWI NG Philosophy

KIWI NG (Next Generation) is a powerful command-line tool for creating Linux system appliances and images.  
It helps you build ready-to-deploy OS images tailored to specific use cases, whether virtual machines, live ISOs, or cloud images.

## 📚 Contents

- [What is KIWI NG?](#what-is-kiwi-ng)
- [Key Features](#key-features)
- [Installation](#installation)
- [Test Image Build](#test-image-build)
<!-- - [Basic Workflow](#basic-workflow)
- [Appliance Descriptions](#appliance-descriptions)
- [Building Images](#building-images)
- [Supported Image Types](#supported-image-types)
- [Useful Commands](#useful-commands) -->
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

I tried multiple ways to install KIWI NG on my Ubuntu machine, but faced a lot of issues. KIWI NG is available in the package manager of OpenSUSE. So, I decided to create a container running OpenSUSE inside, with Docker. The installation is with respect to it:

**Step 1:** Copy the following Dockerfile. (It is custom made to install KIWI NG). Essentially, it uses OpenSUSE as a base image, installs the packages required to test KIWI NG, and clones the KIWI NG Git repository inside it. The shell starts by default, once the container runs.

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

**Step 3:** Run a Docker container using the image in `privileged` mode. You will be taken inside the OpenSUSE shell as `root`.

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
2. Started a Docker container with OpenSUSE Leap as its OS
3. Built a raw image using KIWI
4. Created a QEMU VM inside the container using the image
5. Booted another OpenSUSE Leap OS inside the QEMU VM

Here's a visualization for better understanding:

Host Machine (Ubuntu)
└── Docker Container (openSUSE Leap)
    ├── KIWI NG (builds `.raw` image)
    └── QEMU Virtual Machine
        └── KIWI-built OS (from `.raw` disk image)

---

---

## Resources & Documentation

This is the craziest technology I have ever worked on. It has so little documentation on the Internet that learning it gives you a headache. I found no YouTube videos or other resources on usual websites. My only source was the [official documentation](https://osinside.github.io/kiwi/index.html), and some help from ChatGPT for explaining the entire ordeal.
