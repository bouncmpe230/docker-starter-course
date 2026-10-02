# CMPE230 Docker Container

This README explains how to use the Docker environment provided for **CMPE230**, including:

- how Docker differs from a Virtual Machine (VM),
- the difference between a Docker **image** and a **container**,
- how to create, start, stop, and enter the CMPE230 container.

The Docker image for this course is available on Docker Hub as:

```text
gokceuludogan/cmpe230-spring26
```

---

# Docker vs. Virtual Machines

Docker containers and Virtual Machines can both provide isolated environments, but they work at different levels.

![Docker vs. Virtual Machines](https://camo.githubusercontent.com/3d9025dab9e0e5184873ca5c817ccde6ed2e1da83d9fbc8e600325c2c12ca45d/68747470733a2f2f7777772e66726565636f646563616d702e6f72672f6e6577732f636f6e74656e742f696d616765732f73697a652f77313630302f323032322f31302f332e706e67)

A simplified comparison is:

| | Docker Container | Virtual Machine |
|---|---|---|
| **What is isolated?** | Processes and their environment | An entire computer |
| **Kernel** | Usually shared with other containers | Each VM has its own kernel |
| **Guest OS** | No complete guest OS required | Runs a complete guest OS |
| **Startup** | Usually very fast | Requires booting an OS |
| **Resource usage** | Lower | Higher |
| **Isolation** | Process-level isolation | Stronger OS-level isolation |

## Virtual Machines

A Virtual Machine behaves like a separate computer.

For example:

```text
Physical computer
       ↓
Host operating system
       ↓
Hypervisor
       ↓
Virtual Machine
       ↓
Guest operating system
       ↓
Applications
```

A **hypervisor** is the software responsible for creating and running Virtual Machines. Examples include VirtualBox, VMware, and other virtualization systems.

Each VM runs its own operating system and therefore has its own kernel.

For example:

```text
Windows
   ↓
VirtualBox
   ↓
Ubuntu VM
   ↓
Linux kernel
   ↓
Programs
```

Because a complete operating system must run inside each VM, Virtual Machines normally require more memory, disk space, and startup time.

## Docker Containers

Docker works differently.

A container does not normally contain its own operating-system kernel. Instead, multiple containers can use the same Linux kernel while keeping their processes, filesystems, and networking isolated.

On Linux, the structure is roughly:

```text
Linux
  ↓
Linux kernel
  ↓
Docker
  ├── Container A
  ├── Container B
  └── Container C
```

The containers are isolated from each other, but they share the same underlying kernel.

This is why containers are generally lighter and faster to start than Virtual Machines.

> **Note:** On a Linux computer, Linux containers can directly use the host's Linux kernel. On macOS and Windows, Docker Desktop normally runs a small Linux Virtual Machine in the background because macOS and Windows do not provide a Linux kernel.

For example, on macOS:

```text
Mac hardware
     ↓
   macOS
     ↓
Docker Desktop
     ↓
Small Linux VM
     ↓
Linux kernel
     ↓
CMPE230 container
```

Docker Desktop manages this VM automatically; you normally do not need to interact with it.

---

# Docker Images vs. Containers

Two of the most important Docker concepts are **images** and **containers**.

## Docker Image

A Docker **image** is a template used to create containers.

It contains the files needed for the environment, such as:

- Ubuntu system files,
- compilers,
- libraries,
- development tools,
- configuration files.

For CMPE230, the image is:

```text
gokceuludogan/cmpe230-spring26:latest
```

The image itself is not a running environment.

## Docker Container

A Docker **container** is a running or stopped instance created from an image.

For example:

```text
CMPE230 image
      ↓
docker run
      ↓
CMPE230 container
```

You can create multiple containers from the same image:

```text
CMPE230 image
    ├── Container 1
    ├── Container 2
    └── Container 3
```

Each container gets its own writable filesystem layer, so changes made inside one container do not modify the original image or the other containers.

A useful analogy is:

```text
Image      → Class / Blueprint
Container  → Object / Instance
```

---

# Prerequisites

Make sure Docker is installed and running.

- **Windows/macOS:** Install Docker Desktop.
- **Linux:** Install Docker Engine.

See the official Docker documentation:

[Docker installation instructions](https://docs.docker.com/engine/install/)

You can verify your installation with:

```bash
docker --version
```

---

# Getting Started

## 1. Download the CMPE230 Image

First, download the course image from Docker Hub:

```bash
docker pull gokceuludogan/cmpe230-spring26:latest
```

`docker pull` downloads the image to your computer.

Conceptually:

```text
Docker Hub
    ↓
 docker pull
    ↓
Your computer
    ↓
CMPE230 image
```

You usually need to pull the image only once, unless a newer version is published.

---

# 2. View Downloaded Images

To see the Docker images available on your computer:

```bash
docker images
```

You should see an entry similar to:

```text
REPOSITORY                         TAG       IMAGE ID       SIZE
gokceuludogan/cmpe230-spring26    latest    ...            ...
```

---

# 3. Create and Start the CMPE230 Container

Run:

```bash
docker run -it -d --name cmpe230 gokceuludogan/cmpe230-spring26:latest
```

This command creates a new container from the CMPE230 image and starts it.

The options mean:

```text
-it             prepare an interactive terminal
-d              run the container in the background
--name cmpe230  give the container the name "cmpe230"
```

The relationship is:

```text
CMPE230 image
      ↓
 docker run
      ↓
Container named "cmpe230"
```

> `docker run` creates a **new container**. You normally use this command only the first time you create your `cmpe230` container.

---

# `run` vs. `create` vs. `start`

These commands refer to different stages of a container's lifecycle.

## `docker create`

Creates a new container but does **not** start it.

```bash
docker create --name cmpe230 gokceuludogan/cmpe230-spring26:latest
```

Conceptually:

```text
Image
  ↓
docker create
  ↓
Stopped container
```

---

## `docker start`

Starts an **existing** stopped container.

```bash
docker start cmpe230
```

Conceptually:

```text
Stopped container
       ↓
 docker start
       ↓
Running container
```

---

## `docker run`

`docker run` is essentially:

```text
docker create
      +
docker start
```

So:

```bash
docker run ...
```

means:

```text
Image
  ↓
create container
  ↓
start container
```

The important difference is:

```text
docker run     → creates a NEW container and starts it
docker start   → starts an EXISTING container
```

If you already have a container named `cmpe230`, you should normally use:

```bash
docker start cmpe230
```

rather than running `docker run` again.

---

# 4. View Containers

To show only currently running containers:

```bash
docker ps
```

To show **all containers**, including stopped containers:

```bash
docker ps -a
```

For example:

```text
CONTAINER ID   IMAGE                              STATUS        NAMES
abc123         cmpe230-spring26:latest            Up 2 min      cmpe230
```

The `STATUS` column tells you whether the container is currently running or stopped.

---

# 5. Enter the CMPE230 Container

If the container is running, the easiest way to open a shell inside it is:

```bash
docker exec -it cmpe230 bash
```

You are now running `bash` **inside the CMPE230 Linux container**.

Conceptually:

```text
Your terminal
     ↓
docker exec
     ↓
CMPE230 container
     ↓
bash
```

You can now run Linux commands such as:

```bash
pwd
ls
gcc --version
uname -a
ls /proc
```

Type:

```bash
exit
```

to leave the shell.

Leaving this shell does **not necessarily stop the container**, because the container itself may still be running in the background.

---

# 6. Stop the Container

To stop the running container:

```bash
docker stop cmpe230
```

This does not delete the container.

Its filesystem and other container state remain available.

Conceptually:

```text
Running container
       ↓
 docker stop
       ↓
Stopped container
```

You can later start the same container again using:

```bash
docker start cmpe230
```

---

# 7. Start and Re-enter an Existing Container

If you return to your computer later and your container is stopped, use:

```bash
docker start cmpe230
```

Then enter it with:

```bash
docker exec -it cmpe230 bash
```

So your usual workflow after the initial setup will be:

```bash
docker start cmpe230
docker exec -it cmpe230 bash
```

When you are finished:

```bash
exit
docker stop cmpe230
```

---

# `docker exec` vs. `docker attach`

You may also see:

```bash
docker attach cmpe230
```

`docker attach` connects your terminal directly to the container's main process.

For interactive development, however, we recommend:

```bash
docker exec -it cmpe230 bash
```

because it creates a new shell inside the running container without attaching directly to its main process.

In most CMPE230 exercises, **use `docker exec` to enter your container**.

---

# Container Lifecycle

The full lifecycle can be summarized as:

```text
                    docker pull
Docker Hub ─────────────────────────→ Image
                                         │
                                         │ docker run
                                         │
                                         ↓
                                   Running Container
                                      │        ↑
                          docker stop │        │ docker start
                                      ↓        │
                                   Stopped Container
```

Or, more simply:

```text
IMAGE
  │
  │ docker run
  ↓
RUNNING CONTAINER
  │
  │ docker stop
  ↓
STOPPED CONTAINER
  │
  │ docker start
  ↓
RUNNING CONTAINER
```

---

# Common Commands

| Task | Command |
|---|---|
| Download the CMPE230 image | `docker pull gokceuludogan/cmpe230-spring26:latest` |
| List downloaded images | `docker images` |
| Create and start the container | `docker run -it -d --name cmpe230 gokceuludogan/cmpe230-spring26:latest` |
| List running containers | `docker ps` |
| List all containers | `docker ps -a` |
| Start an existing container | `docker start cmpe230` |
| Enter a running container | `docker exec -it cmpe230 bash` |
| Stop the container | `docker stop cmpe230` |

---

# Summary

The most important ideas are:

```text
Docker Image
    A template containing the CMPE230 environment.

Docker Container
    An instance created from that image.

docker pull
    Downloads an image.

docker run
    Creates a new container and starts it.

docker start
    Starts an existing stopped container.

docker stop
    Stops a running container.

docker exec
    Runs a command, such as bash, inside a running container.
```

For CMPE230, your normal workflow is:

### First time

```bash
docker pull gokceuludogan/cmpe230-spring26:latest

docker run -it -d \
    --name cmpe230 \
    gokceuludogan/cmpe230-spring26:latest
```

### Later sessions

```bash
docker start cmpe230
docker exec -it cmpe230 bash
```

### When finished

```bash
exit
docker stop cmpe230
```
