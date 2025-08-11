# 🐳 Docker

> _"Build once, run anywhere."_ — Docker Philosophy

Docker is a platform for building, packaging, and running applications inside lightweight, portable containers.  
It enables developers to create consistent environments across development, testing, and production — eliminating the infamous “it works on my machine” problem.

## 📚 Contents

- [Docker Architecture](#architecture)
- [Terminology](#terminology)
- [Basic Commands](#basic-commands)
- [Docker Images](#docker-images)
    - [Dockerfile](#dockerfile)
- [Docker Containers](#docker-containers)

By isolating applications and their dependencies, Docker streamlines deployment, improves scalability, and integrates seamlessly into modern DevOps workflows.  
Whether you’re running microservices, experimenting with a new stack, or managing production workloads, Docker makes environment management faster, more predictable, and more reproducible.

---

## Architecture

![Docker Architecture](https://media.geeksforgeeks.org/wp-content/uploads/20221205115118/Architecture-of-Docker.png "Docker Architecture")

---

## Terminology

- **Docker Daemon** – Manages all services by communicating with other daemons and handles objects like containers, images, networks, and volumes.  
- **Docker Client** – Allows users to interact with Docker, sending terminal commands to the Docker daemon.  
- **Docker Host** – A machine responsible for running multiple containers.  
- **Docker Registry** – Also known as Docker Hub, a public repository that stores all Docker images.  
- **Docker Images** – Read-only templates containing information to create Docker containers.  
- **Docker Containers** – Running instances of Docker images, accessing only resources defined in the image unless additional access is granted during build.  

---

## Basic Commands

1. `docker run image`: Starts a container from an image
2. `docker ps (-a)`: Lists containers (`-a` tag lists all the containers, even the stopped ones)
3. `docker stop container-id`: Stop a container
4. `docker rm container-id`: Remove a container
5. `docker images`: Lists images
6. `docker rmi image-id`: Removes an image
7. `docker exec -it container bash`: Executes a command in a running container. *This particular command starts an interactive bash pseudo-terminal inside a container*
8. `docker logs container`: Displays container logs

---

## Docker Images

A Docker image is made up of layers. Each instruction in the image file is a new read-only layer. These instructions are written in a `Dockerfile`. The image is then created by running the command:
```bash
docker build -t image-with-tag . 
```

An image should always be tagged using the `-t` flag during building. For example, `docker build -t my-image:1.0`, where `1.0` is the tag. The default tag is `:latest`, which should NOT be used for production.

Docker images can be uploaded or taken from the Docker Registry as follows:
```bash
docker push my-image:1.0            # Pushes to Docker Hub
docker pull my-image:1.0            # Pulls from Docker Hub
```

Use `.dockerignore` to avoid copying unncessary files

### Dockerfile
A Dockerfile is a blueprint of your image

Common instructions in a Dockerfile are:
| Instruction  | Purpose |
|--------------|---------|
| FROM         | Set the base image to use for the build |
| RUN          | Execute commands at build time to install packages or perform actions |
| CMD          | Provide default command/arguments when the container runs (can be overridden) |
| ENTRYPOINT   | Specify the main command that always runs when the container starts |
| COPY         | Copy files/folders from the build context into the image |
| ADD          | Copy files like COPY, but also supports URLs and auto-extracts archives |
| WORKDIR      | Set the working directory for subsequent instructions (Creates it if missing) |
| ENV          | Define environment variables inside the image |
| EXPOSE       | Document port(s) the container listens on (does not publish them) |
| VOLUME       | Create a mount point for persistent/external data |
| ARG          | Define build-time variables |
| LABEL        | Add metadata (author, version, description) to the image |
| USER         | Set the user to run subsequent commands |
| HEALTHCHECK  | Define how to check if the container is healthy |
| SHELL        | Change the default shell used for RUN commands |

---

## Docker Containers
A container is a running instance of a Docker image. The following is the lifecycle of a container:

> **Create → Start → Pause/Unpause → Stop → Remove**

- Create and Start are done simultaneously during the `docker run image-name` command
- To freeze all the processes in a container, `docker pause container-id` is used
- `docker stop container-id` stops the container
- A Docker container is deleted (removed) with the `docker rm container-id` command

**💡 Key point:** A container only lives as long as its main process is running. Once that process exits, the container stops even if there are background processes running.