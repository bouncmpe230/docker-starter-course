
# CMPE230 Spring 2024 Docker Container
This README provides a overview of using the Docker container for CMPE230, Spring 2024 edition, along with a comparison between Docker and Virtual Machines (VMs), and a  explanation of Docker containers and images. The Docker image for this course is hosted on Docker Hub under `gokceuludogan/cmpe230_spring24`.

## Comparing Docker with Virtual Machines (VMs)

![](https://www.freecodecamp.org/news/content/images/size/w1600/2022/10/3.png)
- **Architecture**: Docker utilizes containerization to run applications in isolated environments, sharing the host system’s kernel. In contrast, VMs emulate entire hardware stacks, each running a separate operating system.
- **Resource Efficiency**: Docker containers are more resource-efficient than VMs, as they share the host's kernel and avoid running multiple OS instances.
- **Performance**: Containers typically have less overhead compared to VMs, offering improved performance.
- **Isolation**: While VMs provide strong isolation by completely separating instances, Docker containers offer process-level isolation, which is generally secure but slightly less isolated compared to VMs.

## Understanding Docker: Containers vs. Images

Understanding the difference between Docker containers and Docker images is crucial in Docker technology. Here's a concise comparison:

- **Docker Images**: These are immutable **templates** used to create containers. They include the application code, runtime, libraries, and settings.
- **Docker Containers**: **Runnable instances** of Docker images. They are mutable, meaning that they can be started, stopped, moved, and deleted.

## Prerequisites

Ensure Docker is installed on your system. Visit [Docker's official website](https://www.docker.com/) for installation instructions.

## Getting Started with the Docker Container

*  **Pulling the Docker Image**
```bash
docker pull gokceuludogan/cmpe230_spring24:latest
```

* **Running the Docker Container**
```bash
docker run -it -d --name cmpe230 gokceuludogan/cmpe230_spring24:latest
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





