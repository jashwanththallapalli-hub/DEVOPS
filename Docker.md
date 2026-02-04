# DOCKER NOTES

## WHAT IS DOCKER?

Docker is an open-source containerization platform that allows developers to build, ship, and run applications in isolated environments called containers.

## WHAT ARE CONTAINERS?

A container is like a small box where your application lives along with everything it needs (like libraries, configuration, runtime, etc.). Because of this, the application will run the same everywhere — on your laptop, another computer, or a cloud server.

Also defined as **RUNNING INSTANCE OF AN IMAGE**.

## WHAT IS IMAGE?

Docker image is a template used to create containers.

## WHAT IS DOCKER FILE?

A Dockerfile is a text file that is used to build an image with a set of instructions.

## VOLUME

A volume is a storage method in Docker used to save data so it is not lost when a container stops or is removed.

## VIRTUAL MACHINE

Virtual machine is like a computer inside the computer.

## SAMPLE IMAGE OF DOCKER FILE

<img width="301" height="167" alt="image" src="https://github.com/user-attachments/assets/960b9a40-9ed7-4b3f-908b-1e950f92ab3f" />



## DOCKER FILE → IMAGE → CONTAINER

<img width="373" height="135" alt="image" src="https://github.com/user-attachments/assets/c6d9dc56-883d-450c-a58a-00509a3160d8" />



## DIFFERENCE BETWEEN DOCKER & VIRTUAL MACHINE

<img width="318" height="159" alt="image" src="https://github.com/user-attachments/assets/5ce2cf74-d526-4923-9139-4e019fa9674e" />



### BARE METAL

Bare metal refers to physical hardware where the operating system runs directly on the physical server, without any virtualization layer.

**Example:** It's like using a computer directly without sharing it.

**Simple words:** An operating system that is directly installed on the hardware without hypervisors or virtual machines.

## WHY IS DOCKER USED?

It solved one of the biggest problems: **"It's working on my machine, but not on yours"**

### DOCKER ENSURES:

- **PORTABILITY:** We can move containers anywhere - laptop → server → cloud
- **CONSISTENCY:** The app runs the same in development, testing, and production
- **ISOLATION:** Each container runs independently (no conflicts between apps)
- **SPEED:** Containers start faster than virtual machines

## WHERE IS DOCKER USED?

- **Software development** – for creating consistent environments
- **Testing** – to test applications in different configurations easily
- **Deployment** – for CI/CD (Continuous Integration/Deployment) pipelines
- **Cloud computing** – almost every major cloud (AWS, Azure, GCP) supports Docker
- **Microservices architecture** – running multiple small services independently

## OCI - Open Container Initiative

It is a standard that defines how container images and container runtimes should be built and executed.

### Why was OCI created?

Before OCI was created, different companies were creating their own container formats which resulted in incompatibility. To solve this, many companies came together and created OCI so that all containers follow the same rules.

OCI makes sure that containers built using Docker can run anywhere - in Kubernetes or cloud platforms.

## WHAT ARE UNDERLYING TECHNOLOGIES IN DOCKER?

Actually, Docker follows several Linux kernel features that provide isolation, resource control, and file system layering.

### NAMESPACES

Namespaces make each container feel like it is running on its own independent system, even though multiple containers share the same host OS.

Namespaces in Docker ensure that each container has its own isolated environment. They control what a container can see in the system, such as:

- Its own process list
- Its own network (IP & ports)
- Its own file system
- Its own hostname
- Its own users and permissions

This isolation prevents one container from interfering with another or with the host system.

### CGROUPS (Control Groups)

Cgroups are responsible for how much resources a container can use. They limit:

- CPU usage
- RAM (Memory) usage
- Disk I/O
- Network bandwidth

So one container cannot take all system resources and crash other containers.

### UNION FILE SYSTEM

A Union File System allows multiple layers of files to be combined into a single view. Docker uses this to build images in layers.

**Best example:** Burger - it contains multiple layers but overall we call it a burger.

**Summary:**
- **Namespaces** → What a container can SEE
- **cgroups** → What a container can USE
- **UnionFS** → How files are STORED and SHARED in layers

### CONTAINERD

containerd is a container runtime manager that handles the complete lifecycle of containers.

It is responsible for:
- Pulling images
- Managing container storage
- Starting and stopping containers
- Managing container networks
- Talking to lower-level runtimes (like runc)

### RUNC

runc is a low-level container runtime that actually creates and runs containers according to the OCI (Open Container Initiative) runtime specification.

## DOCKER ARCHITECTURE

<img width="309" height="163" alt="image" src="https://github.com/user-attachments/assets/fcc2837d-a1d1-4439-a538-a19baf9b9fec" />



## BASIC DOCKER COMMANDS

```bash
# Download an image from a registry, like Docker Hub
docker pull <image>

# Build an image from a Dockerfile
docker build -t <image name> <path>

# List all images available on your local machine
docker image ls

# Run a container from an image, mapping host ports to container ports
docker run -d -p <host port>:<container port> --name <container name> <image>

# List all running containers
docker container ls

# Stop a running container
docker container stop <container>

# Remove a stopped container
docker container rm <container>

# Remove an image from your local machine
docker image rm <image>
```

## DATA PERSISTENCE

Data Persistence in Docker means storing data in a way that it remains available even after the container is stopped, restarted, or deleted.

By default, data inside a container is temporary and gets deleted when the container is removed. So we use persistent storage methods to keep the data safe.

### WHY DO WE NEED DATA PERSISTENCE?

Because containers are:
- **Ephemeral** (temporary)
- **Stateless** by default

If a container stores data only inside itself, the data disappears when:
- Container stops
- Container restarts
- Container is recreated (which happens often in deployments)

**Example:**
If a MySQL container stores database data inside itself → Deleting the container → all database tables are lost

So we need persistence to protect data.

### EPHEMERAL CONTAINER FILE SYSTEM

In every container there is a temporary writable layer. Any file saved there will be lost if the container stops running.

Docker provides two ways to make data persistent:
1. Volume mount
2. Bind mounts

### VOLUMES

A Volume is a storage space created and managed by Docker outside the container.

**Why Use?**
- Prevent data loss
- Works well with databases & production apps
- Containers can be removed without losing data

**Volume Commands:**

```bash
# Create a volume
docker volume create myvolume

# List all volumes
docker volume ls

# Inspect a volume (see where it is stored)
docker volume inspect myvolume

# Remove a specific volume
docker volume rm myvolume

# Remove all unused volumes
docker volume prune

# Run a container and attach a volume
docker run -d -v myvolume:/data busybox

# Run a container with a bind mount (host folder → container folder)
docker run -d -v /path/on/host:/path/in/container nginx

# Check volume usage in the container
docker inspect container_name
```

### BIND MOUNTS

Bind Mounts use a specific directory on your host machine and map it into the container.

**Why Use?**
- Useful during development
- Supports hot reloading

### When to Use Which

<img width="275" height="183" alt="image" src="https://github.com/user-attachments/assets/c0d0a8cd-ae1e-4572-9003-71c7a8b7ff97" />


## Using Third-Party Container Images

### Definition

These are pre-built container images available on registries like:
- Docker Hub
- GitHub Container Registry
- AWS ECR
- GCP GCR

## Running Containers

The `docker run` command creates and starts containers from images in one step. It combines `docker create` and `docker start` operations, allowing you to execute applications in isolated environments with various configuration options like port mapping, volumes, and environment variables.

### DOCKER RUN

`docker run` is used to start a single container from a Docker image.

**Meaning:** It runs one container at a time.

**When to Use docker run:**
- Running a single service
- Quick testing
- Simple applications

### DOCKER COMPOSE

`docker compose` is used to run multiple containers together, using a configuration file called `docker-compose.yml`.

**Meaning:** It manages multi-container applications with a single command. Everything runs as a service.

**When to Use docker compose:**
- Multi-container applications
- Development environments
- Complex application stacks

## What are Container Registries?

A Container Registry is a storage and distribution system for Docker images. It is like a storehouse where Docker images are uploaded (push) and downloaded (pull).

**In simple words:** A Container Registry is a place where Docker images are stored and managed.

### Why Do We Need Container Registries?

Because applications are shared and deployed across:
- Developer machines
- Servers
- Cloud environments
- CI/CD pipelines

So, instead of sending code, we package the application as an image, upload it to a registry, and then download it wherever needed.

### Two Types of Container Registries

1. **Public Registries** - Anyone can access
2. **Private Registries** - Restricted access

### Popular Container Registries

- Docker Hub
- GitHub Container Registry
- AWS ECR
- Google Container Registry (GCR)
- Azure Container Registry (ACR)

### How It Works (Push & Pull)

**Upload to registry:**
```bash
docker push <registry>/<image_name>:<tag>
```

**Download from registry:**
```bash
docker pull <registry>/<image_name>:<tag>
```

**Example:**
```bash
docker pull nginx
```

This pulls nginx image from Docker Hub to your system.

### Image Tagging Best Practice

Always tag image versions:
- `myapp:v1`
- `myapp:v2`

**Avoid using:** `latest` because it can cause confusion about versions in deployment.

## Building Container Images

A container image is a package that contains:
- Your application code
- Required dependencies
- Libraries
- System tools
- Runtime environment

When you run an image → it becomes a container.

To build images in Docker, we use **Dockerfiles**.

### Dockerfiles

A Dockerfile is a text file that has step-by-step instructions for building an image.

**Example Dockerfile:**
```dockerfile
FROM python:3.10          # Base Image
WORKDIR /app              # Set working directory inside container
COPY requirements.txt .   # Copy dependencies file
RUN pip install -r requirements.txt  # Install dependencies
COPY . .                  # Copy application code
CMD ["python", "main.py"] # Command to run when container starts
```

### Key Instructions:

- `FROM` - Base image
- `WORKDIR` - Working directory
- `COPY` - Copy files
- `RUN` - Execute commands
- `CMD` - Default command to run
- `EXPOSE` - Expose ports
- `ENV` - Environment variables

## Efficient Layer Caching

Docker builds images in layers — each instruction in the Dockerfile creates a new layer.

### Why does this matter?

Docker reuses layers when building, so rebuilding is faster.

**Example:**
```dockerfile
FROM python        ← Layer 1 (cached)
RUN pip install .. ← Layer 2 (cached)
COPY . .           ← Only this part rebuilds
```

**Benefits:**
- Faster builds
- Saves disk storage
- Improves CI/CD pipeline speed

## Image Size and Security

### Image Size Matters

Smaller images:
- Pull faster
- Deploy faster
- Use less disk space

**How to reduce size:**
- Use minimal base images (alpine)
- Multi-stage builds
- Remove unnecessary files

### Image Security

A secure image must be:
- Free of vulnerabilities (scan before use)
- Built from trusted base images
- Not running as root inside the container

**Best Practices:**
- Scan images regularly
- Use official images
- Keep images updated
- Don't store secrets in images

## DOCKER CLI

Docker CLI is the command-line tool used to interact with the Docker Daemon for managing images, containers, networks, and volumes.

### How it works internally

```
You (developer)
   ↓ commands
Docker CLI (docker command)
   ↓ talks to
Docker Daemon (dockerd)
   ↓ controls
Images, Containers, Networks, Volumes
```

## Hot Reloading

### What it is:

Hot Reloading means when you change your source code, the changes automatically reflect inside the running container without rebuilding the image.

### How it works:

Usually done with Bind Mounts (mapping local folder to container folder):
```bash
docker run -v /mycode:/app myimage
```

### Why it helps:

Hot Reload = Faster coding

## Debuggers

### What it is:

Debuggers let developers pause code execution, check variable values, and step through code inside the container.

### How it works:

Expose debugger port from container → IDE connects to it.

**Example:**
```bash
docker run -p 5678:5678 myapp
```

### Why it helps:

- Makes debugging inside container the same as local machine
- No need to run code outside Docker
- Debugger = Easy bug fixing

## Tests Inside Containers

### What it is:

Running tests inside Docker ensures the application behaves the same in:
- Developer laptop
- Test environment
- Production servers

**Testing in containers = Reliability + No environment mismatch**

## Continuous Integration (CI)

### What it is:

CI means every time code is pushed to GitHub/GitLab:
- The system automatically builds the app
- Runs tests
- Scans images
- And confirms the code is safe to deploy

**Tools:**
- GitHub Actions
- GitLab CI/CD
- Jenkins
- Bitbucket Pipelines

### Why it helps:

- No need to test manually
- Ensures code quality
- Prevents bugs from going to production

**CI = Automated testing + Code quality protection**

## Deploying Containers

Once we build and run containers, the next step is deploying them on servers or cloud. But in real applications, we usually have many containers (web app, database, cache, API services, etc.).

We need tools to manage, scale, restart, load balance, and monitor these containers. These tools are called **container orchestrators**.

### Nomad

Nomad is a simple and lightweight container orchestration tool created by HashiCorp.

**Key points:**
- Easier to learn than Kubernetes
- Can run containers and non-container workloads
- Works well with Consul and Vault
- Good for small-to-medium companies

**Where it's used:**
- Internal applications
- Flexible environments

### Docker Swarm

Docker Swarm is Docker's built-in orchestration tool.

**Key points:**
- Very easy to set up
- Uses Docker CLI commands (so familiar)
- Provides clustering + load balancing
- But not as powerful or flexible as Kubernetes

**When to use:**
- Small to medium deployments
- Simple orchestration needs

### Kubernetes (K8s) — Most Important

Kubernetes is the industry standard for container orchestration.

**Key points:**
- Automatically starts/stops/restarts containers
- Can scale up/down containers based on load
- Provides self-healing (if container crashes, Kubernetes restarts it)
- Supports advanced networking and security
- Works across cloud platforms (AWS, Azure, GCP)

**Used by:**
- Google
- Netflix
- Amazon
- Almost every large company

**When to use:**
- Production environments
- Large-scale applications
- Enterprise deployments

## PaaS Options (Platform as a Service)

Instead of managing infrastructure yourself, you can let cloud platforms run containers for you.

**Examples:**
- AWS ECS/Fargate
- Google Cloud Run
- Azure Container Instances
- Heroku

**Why use PaaS?**
- No need to configure servers
- Automatic scaling
- Faster deployment

**ORCHESTRATION:** Arranging or managing multiple things in a proper order so that everything works together smoothly to produce a good result.

## Docker Swarm – What It Does

Docker Swarm allows you to connect multiple servers (nodes) together to form one cluster, and then run containers across all those servers as if they were on a single machine.

This means you can have:
- Multiple machines working together
- Load balancing across nodes
- High availability

So Swarm turns many servers → into one big virtual Docker host.

### Why is This Useful?

Because:
- You can deploy multiple containers across multiple servers
- You can scale applications easily
- You get high availability (if one machine goes down, others take over)

## Docker Commands Reference

<img width="1102" height="777" alt="image" src="https://github.com/user-attachments/assets/6af4a409-9ea5-4ba8-8b9d-133f6cfef29c" />



---

## Notes for Images

This Markdown file contains placeholders for the following images from the original document. Please add these images to an `images` folder in your Git repository:

1. `dockerfile-sample.png` - Sample Dockerfile image
2. `docker-flow.png` - Docker File → Image → Container flow diagram
3. `docker-vs-vm.png` - Comparison between Docker and Virtual Machine
4. `docker-architecture.png` - Docker architecture diagram
5. `volumes-vs-bind-mounts.png` - Comparison table for volumes vs bind mounts
6. `docker-commands.png` - Docker commands reference

🔗 **Docker Cheat Sheet**  
https://docs.docker.com/get-started/docker_cheatsheet.pdf
