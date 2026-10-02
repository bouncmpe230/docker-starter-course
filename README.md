
# CMPE230 Docker Container
This README provides an overview of using the Docker container for CMPE230, compares Docker with Virtual Machines (VMs), and explains Docker containers and images. The Docker image for this course is hosted on Docker Hub under `gokceuludogan/cmpe230-spring26`.

## Comparing Docker with Virtual Machines (VMs)

[image](https://camo.githubusercontent.com/3d9025dab9e0e5184873ca5c817ccde6ed2e1da83d9fbc8e600325c2c12ca45d/68747470733a2f2f7777772e66726565636f646563616d702e6f72672f6e6577732f636f6e74656e742f696d616765732f73697a652f77313630302f323032322f31302f332e706e67)

- **Architecture**: Virtual Machines virtualize hardware through a hypervisor. Each VM runs its own operating system, including its own kernel. Docker containers instead isolate applications at the operating-system level. Multiple containers can share the same kernel while having their own isolated processes, filesystems, and network environments.

- **Resource Efficiency**: Containers are generally more resource-efficient than VMs because they do not require a complete guest operating system for each application. Multiple containers can share the same underlying kernel.

- **Performance**: Containers usually have lower CPU, memory, and startup overhead than VMs. A container can often start in seconds or less because it does not need to boot an entire operating system.

- **Isolation**: VMs provide a stronger isolation boundary because each VM has its own kernel. Containers isolate processes using operating-system mechanisms such as namespaces and cgroups, but containers running on the same host normally share a kernel.

> **Note:** On a Linux host, Linux Docker containers can directly share the host's Linux kernel. On macOS and Windows, Docker Desktop typically runs Linux containers inside a lightweight Linux Virtual Machine because these systems do not provide a Linux kernel directly.

## Understanding Docker: Containers vs. Images

Understanding the difference between Docker containers and Docker images is crucial in Docker technology. Here's a concise comparison:

- **Docker Images**: These are immutable **templates** used to create containers. They include the application code, runtime, libraries, and settings.
- **Docker Containers**: **Runnable instances** of Docker images. They are mutable, meaning that they can be started, stopped, moved, and deleted.

## Prerequisites

Ensure Docker is installed on your system. Visit [Docker's official website](https://www.docker.com/) for installation instructions.

## Getting Started with the Docker Container

*  **Pulling the Docker Image**
```bash
docker pull gokceuludogan/cmpe230-spring26:latest
```

* **Running the Docker Container**
```bash
docker run -it -d --name cmpe230 gokceuludogan/cmpe230-spring26:latest
```

This command runs the container in detached mode (-d), with an interactive terminal (-it), and names the container `cmpe230`.

* **run vs. create vs. start**

- **docker run**: This command is used to create a new container and start it immediately. It's a combination of `docker create` and `docker start`. Use this when you want to create and run a container in one step.
- **docker create**: This command creates a new container but does not start it. It's useful when you want to set up a container in advance and start it later.
- **docker start**: This command starts a container that was created and stopped. Use this to restart a container that was previously created and stopped.

* **Listing Docker Images**

```bash
docker images
```

* **Viewing Active Containers**

```bash
docker ps -a
```
*  **Stopping the Container**
```bash
docker stop cmpe230
```
* **Attaching to the Container**
After stopping the container, you can attach to it with:
```bash
docker start cmpe230
docker attach cmpe230
```





