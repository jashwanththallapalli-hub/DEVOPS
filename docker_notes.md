# DOCKER NOTES

## WHAT IS DOCKER ?

Docker is an open-source containerization platform that allows developers to build, ship, and run applications in isolated environments called containers.

## WHAT ARE CONTAINERS?

A container is like a small box where your application lives along with everything it needs (like libraries, configuration, runtime, etc.).
Because of this, the application will run the same everywhere — on your laptop, another computer, or a cloud server.

Also defined as RUNNING INSTANCE OF AN IMAGE.

## WHAT IS IMAGE?

Docker image is a template used to create an containers.

## WHAT IS DOCKER FILE?

A docker file is a text file that is used to build an image with a set of instructions.

## VOLUME

A volume is a storage method in Docker used to save data so it is not lost when a container stops or is removed.

## VIRTUAL MACHINE

Virtual machine is like a computer inside the computer.

## SAMPLE IMAGE OF DOCKER FILE

![Sample Dockerfile Image](images/sample_dockerfile.png)

## DOCKER FILE-->IMAGE-->CONTAINER

![Dockerfile to Image to Container](images/dockerfile_image_container.png)

## DIFFERENCE BETWEEN THE DOCKER & VIRTUAL MACHINE

![Docker vs VM](images/docker_vs_vm.png)

| | Docker container | VM |
|---|---|---|
| What is it? | Docker is a software platform to create and run Docker containers. A Docker container is an emulation of a user-space instance, the part of the operating system where user processes run. | An emulation of a physical machine—including virtualized hardware—running an operating system. |
| Virtualization | Container abstracts operating system details from the application code. | VM abstracts hardware details from the application code. |
| Objective | Abstract hardware details and increase hardware utilization. | Improve application environment management and bring consistency across multiple environments. |
| Managed by | The Docker Engine coordinates between the operating system and Docker containers. | The hypervisor coordinates between the machine’s physical hardware and virtual machines. |
| Architecture | Shares resources with the underlying host kernel. | Runs its own kernel and operating system. |
| Resource sharing | On-demand. | A fixed amount, set in a virtual machine image’s configuration requirements. |

## BARE METAL

Bare metal refers to physical hardware where the operating system runs directly on the physical server, with out any virtualization layer.

Ex: it’s like using computer directly without sharing it.

Simple words: an operating system that is directly installed on the hardware without hypervisors or virtual machines.

## WHY IS DOCKER USED:

It solved one of the biggest problem “its working on my machine, but not on yours”

## DOCKER ENSURES;

PORTABILITY: we can move containers anywhere laptop--> server-->cloud.

CONSISTENCY: The app runs the same in the development, testing, and production.

ISOLATION: Each containers run independently (no conflicts between the apps).

SPEED: The containers starts faster than the virtual machines.

## WHERE IS DOCKER USED?

Software development – for creating consistent environments.

Testing – to test applications in different configurations easily.

Deployment – for CI/CD (Continuous Integration/Deployment) pipelines.

Cloud computing – almost every major cloud (AWS, Azure, GCP) supports Docker.

Microservices architecture – running multiple small services independently.

## OCI-Open Container Initiative

It is a standard that defines how container images and container runtimes should be built and executed.

Why was OCI created?

Before OCI was created different companies were creating their own container formats which results to an incompatibility.

So to solve this many companies came together and created OCI so that all containers follow the same rules.

So OCI makes sure that containers built using the docker can run any where in Kubernetes or cloud platforms.

## WHAT ARE UNDERLYING TECHNOLOGIES IN DOCKER?

Actually the docker follows the several Linux kernel features that provide isolation, resource control, and file system layering.

### NAMESPACES

Namespaces make each container feel like it is running on its own independent system, even though multiple containers share the same host OS.

Namespaces in Docker ensure that each container has its own isolated environment.
They control what a container can see in the system, such as:

- its own process list
- its own network (IP & ports)
- its own file system
- its own hostname
- its own users and permissions

This isolation prevents one container from interfering with another or with the host system.

### CGROUPS(control groups):

Cgroups are responsible for how much resources does an container can use.

They limit:

- CPU usage
- RAM (Memory) usage
- Disk I/O
- Network bandwidth

So one container cannot take all system resources and crash other containers.

### UNION FILE SYSTEM

A Union File System allows multiple layers of files to be combined into a single view.

Docker uses this to build images in layers.

Best example: Berger it contains multiple layers but overall we call it as burger

Namespaces → What a container can SEE  
cgroups → What a container can USE  
UnionFS → How files are STORED and SHARED in layers

## CONTAINERD

containerd is a container runtime manager that handles the complete lifecycle of containers.

It is responsible for:

- Pulling images
- Managing container storage
- Starting and stopping containers
- Managing container networks
- Talking to lower-level runtimes (like runc)

## Runc:

runc is a low-level container runtime that actually creates and runs containers according to the OCI (Open Container Initiative) runtime specification.

## DOCKER ARCHITECTURE

![Docker Architecture](images/docker_architecture.png)

## BASIC DOCKER COMMANDS

- docker pull <image>: Download an image from a registry, like Docker Hub.
- docker build -t <image name> <path>: Build an image from a Docker file, where <path> is the directory containing the Docker file.
- docker image ls: List all images available on your local machine.
- docker run -d -p <host port>:<container port> --name <container name> <image>: Run a container from an image, mapping host ports to container ports.
- docker container ls: List all running containers.
- docker container stop <container>: Stop a running container.
- docker container rm <container>: Remove a stopped container.
- docker image rm <image>: Remove an image from your local machine.

## DATA PERSISTENCE

Data Persistence in Docker means storing data in a way that it remains available even after the container is stopped, restarted, or deleted.

By default, data inside a container is temporary and gets deleted when the container is removed.
So we use persistent storage methods to keep the data safe.

## WHY DO WE NEED DATA PERSISTENCE

Because containers are:

- Ephemeral (temporary)
- Stateless by default

If a container stores data only inside itself, the data disappears when:

- Container stops
- Container restarts
- Container is recreated (which happens often in deployments)

Example:

If a MySQL container stores database data inside itself →
Deleting the container → all database tables are lost

So we need persistence to protect data

EPHEMERAL CONTAILER FILE SYSTEM-->in every container there is temporary writable layer any file saved there it will be lost if the container stops running.

Here the docker provide two ways to make the data persistent
Volume mount, Bind mounts

## VOLUMES

A Volume is a storage space created and managed by Docker outside the container.

Why Use?

- Prevent data loss
- Works well with databases & production apps
- Containers can be removed without losing data

### Volume Commands

```
docker volume create myvolume
docker volume ls
docker volume inspect myvolume
docker volume rm myvolume
docker volume prune
docker run -d -v myvolume:/data busybox
docker run -d -v /path/on/host:/path/in/container nginx
docker inspect container_name
```

## BIND MOUNTS:

Bind Mounts use a specific directory on your host machine and map it into the container.

Why Use?

- Useful during development
- Supports hot reloading

## When to Use Which

| Situation | Use | Why |
|---|---|---|
| Development | Bind Mount | Code changes reflect instantly |
| Production / Databases | Volume | Safe, stable, Docker-managed |
| Sharing data between containers | Volume | Multiple containers can use same storage |

## Using Third-Party Container Images

Definition

These are pre-built container images available on registries like:

- Docker Hub
- GitHub Container Registry
- AWS ECR
- GCP GCR

## Running Containers

The docker run command creates and starts containers from images in one step. It combines docker create and docker start operations, allowing you to execute applications in isolated environments with various configuration options like port mapping, volumes, and environment variables.

## DOCKER RUN

docker run is used to start a single container from a Docker image.

Meaning:

- It runs one container at a time.

When to Use docker run:

| Situation | Use docker run? |
|---|---|
| Running a single app/service | Yes |
| Quick testing | Yes |
| Local small projects | Yes |
| Running multiple services together (app + database) | Hard / Complicated |

## DOCKER COMPOSE

docker compose is used to run multiple containers together, using a configuration file called docker-compose.yml.

Meaning:

- It manages multi-container applications with a single command.
- In this every thing it runs as a service

## Why Docker Compose is Needed?

Problem without Compose:

If you have:

- Web container
- Database container
- Redis container

You must run all manually with long commands → difficult.

Solution with Compose:

```
docker compose up -d
```

All containers start together, properly connected.

## Where Docker Compose is Used?

- Web development (Flask, Django, Node.js, React)
- Databases (MySQL, MongoDB, PostgreSQL)
- Microservices architecture
- Local development environments
- DevOps automation
- Running multiple dependent services

## How Docker Compose Works

Compose uses a YAML file.
The file defines:

- services
- image
- build
- ports
- volumes
- environment
- depends_on
- networks

Default behavior:
Compose creates a new network so all services can talk using service names (not IPs).

## Most Important Docker Compose Commands

```
docker compose up
docker compose up -d
docker compose down
docker compose down --volumes
docker compose up --build
docker compose build
docker compose ps
docker compose exec <service> bash
```

## What are Container Registries?

A Container Registry is a storage and distribution system for Docker images.

## Two Types of Container Registries

| Type | Examples | Use |
|---|---|---|
| Public Registries | Docker Hub, Quay.io | Anyone can pull images |
| Private Registries | AWS ECR, GCP GCR, Azure ACR, GitHub GHCR, JFrog Artifactory | Used inside companies |

## Popular Container Registries

| Registry | Provider |
|---|---|
| Docker Hub | Docker |
| ECR | AWS |
| GCR | Google |
| ACR | Microsoft Azure |
| GHCR | GitHub |
| JFrog Artifactory | JFrog |

## Building Container Images

A container image is a package that contains:

- Your application code
- Required dependencies
- Libraries
- System tools
- Runtime environment

## Docker files

A Docker file is a text file that has step-by-step instructions for building an image.

```dockerfile
FROM python:3.10
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "main.py"]
```

## Efficient Layer Caching

![Docker Layers](images/docker_layers.png)

## Image Size and Security

### Image Size Matters

Smaller images pull faster and deploy faster.

### Image Security

Use official images, keep updated, do not embed secrets, run as non-root.

## DOCKER CLI

Docker CLI is the command-line tool used to interact with the Docker Daemon.

![Docker CLI Flow](images/docker_cli_flow.png)

## Hot Reloading

![Hot Reloading](images/hot_reloading.png)

## Debuggers

![Debugger](images/debugger.png)

## Tests Inside Containers

![Tests in Containers](images/tests_containers.png)

## Continuous Integration (CI)

![CI Pipeline](images/ci_pipeline.png)

## Deploying Containers

![Container Orchestration](images/container_orchestration.png)

## Nomad

![Nomad](images/nomad.png)

## Docker Swarm

![Docker Swarm](images/docker_swarm.png)

## Kubernetes (K8s)

![Kubernetes](images/kubernetes.png)

## PaaS Options (Platform as a Service)

![PaaS](images/paas.png)

## ORCHESTRATION

Arranging or managing multiple things in a proper order so that everything works together smoothly.

## Docker Swarm – What It Does

![Swarm Architecture](images/swarm_architecture.png)

https://docs.docker.com/get-started/docker_cheatsheet.pdf docker commands
