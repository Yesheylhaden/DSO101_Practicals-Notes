# 🐳 Docker — Getting Started Notes

> 📖 **Source:** [Docker Official Documentation — Get Started](https://docs.docker.com/get-started/)
---

## Table of Contents

1. [What is Docker?](#1-what-is-docker)
2. [Docker Architecture](#2-docker-architecture)
3. [Core Concepts](#3-core-concepts)
   - [Containers](#31-containers)
   - [Images](#32-images)
   - [Registries](#33-registries)
   - [Docker Compose](#34-docker-compose)
4. [Key Docker Commands](#4-key-docker-commands)
5. [Writing a Dockerfile](#5-writing-a-dockerfile)
6. [Docker Compose File Example](#6-docker-compose-file-example)
7. [Quick Reference — Containers vs VMs](#7-quick-reference--containers-vs-vms)

---

## 1. What is Docker?

Docker is a technology that is an open platform for developing, distributing, and operating software.

- This makes it possible to **keep your software development process separate from the infrastructure**.
- It becomes possible to manage your infrastructure like you do applications.
- Brings down the distance between the two extremes - coding and operations.

### Why use Docker?

| Use Case                | Benefit                                       |
| ----------------------- | --------------------------------------------- |
| **Speedy and uniform delivery** | Collaboration among developers using containers; identical environments all round (dev to test to prod) |
| **CI/CD processes**     | Excellent use cases for containerized automated testing and CI/CD |
| **Flexible scaling**    | Scaling up or down almost instantly regardless of whether it is local laptops, data centers, or cloud platforms |
| **Effective hardware utilization** | Alternative lightweight option to VMs that enables you to run more processes on the same hardware |

---

## 2. Docker Architecture

Docker uses a **client–server architecture**.

```
[ Docker Client (docker CLI) ]
          |
          |  REST API (over UNIX socket or network)
          ↓
[ Docker Daemon (dockerd) ]
          |
    ┌─────┴──────┐
    ↓            ↓
[Containers]  [Images / Volumes / Networks]
          |
          ↓
[ Docker Registry (e.g. Docker Hub) ]
```

### Components

- **Docker Daemon (dockerd)**: The service responsible for performing all operations, including building, running, and managing containers. Can also be used to communicate with other daemons to manage Docker services.
- **Docker Client (docker)**: Command-line interface that users use to send Docker API calls (e.g., `docker run`).  
- **Docker Desktop**: An application that combines all the necessary tools to work with Docker (daemons, client, Compose, Kubernetes, etc.). Works on macOS, Windows, Linux.
- **Docker Registries**: Services used to store Docker images. Default one is Docker Hub, but there are also options to host private registries.

### Technical Details

- Language: written in **Go**.
- Containerization technology: based on **Linux kernel namespaces**, which enable container isolation and ensure that each container has its own namespace.
---

## 3. Core Concepts

### 3.1 Containers
> Container is an **isolated and executable process**, which is bundled with all its requirements to run the process such as code, runtime, system tools, libraries, and configuration.

#### Key characteristics of containers:

- **Self-contained**: No requirement of any existing applications on the host machine.
- **Isolated**: Has little effect on the host system or any other containers.
- **Stand-alone**: Every container is independent and can be individually operated.
- **Portability**: Operates in the same manner on any platform whether it is local, cloud, or data center.

#### Containers vs Virtual Machines (VMs)

|| Container | Virtual Machine |
|---|---|---|
| Operating System | Uses the same **kernel of the host machine** | Has a completely separate **OS and kernel** |
| Size | Small (MB) | Large (GB) |
| Startup Time | Seconds | Minutes |
| Isolation Level | Process level | Hardware level |
| Typical Usage | Microservices and apps | OS-based environments |

> **Tip:** VMs and containers work well together – you can run VMs in the cloud that will run containers to run multiple applications at once.

#### Basic container lifecycle:

```bash
# Run a container
docker run -d -p 8080:80 docker/welcome-to-docker

# List running containers
docker ps

# List all containers (including stopped)
docker ps -a

# Stop a container
docker stop <container-id>

# Remove a container
docker rm <container-id>
```

---

### 3.2 Images

A container image is a **standardized and read-only container packaging that contains all files, binaries, libraries, and configuration required to execute a container**.

##### Two significant concepts:

1. **Image immutability**: Once created, an image cannot be modified; changes result in a new image or an extra layer.
2. **Layered architecture of images**: Each layer corresponds to a series of filesystem operations (create, update, or delete files). Only the modified layers are rebuilt, which results in efficient building.

##### Docker Hub image categories:

- **Docker Official Images**: Curated, secure, and broadly used base images (e.g., `ubuntu`, `node`, `python`).
- **Docker Hardened Images**: Minimal and near-CVE-free images for use in production environments.
- **Docker Verified Publishers**: Images provided by verified commercial publishers.
- **Docker-Sponsored Open Source**: Images from the open-source projects supported by Docker.

#### Useful image commands:

```bash
# Search for an image
docker search nginx

# Pull an image
docker pull nginx

# List local images
docker image ls

# View image layers
docker image history nginx

# Remove an image
docker image rm nginx
```

---

### 3.3 Registries

> A registry is a **storage and distribution system** for Docker images.

- **Docker Hub** (`hub.docker.com`) — the default public registry; over 100,000 images available.
- You can run your own **private registry** for internal images.

```bash
# Pull an image from a registry
docker pull nginx

# Push your image to Docker Hub
docker push your-username/your-image:tag
```

---

### 3.4 Docker Compose

> Using Docker Compose allows for the **creation and management of multi-container applications** via a single YAML file (`compose.yaml`).

### Reasons for using Compose

- Start multiple containers in one go (e.g. application + database + cache).
- Specify everything from services to networks and volumes in a single file.
- The person cloning your repository needs just a single command to bring up the entire stack.
- It is **declarative** - state what needs to be done and Compose will work out how to do that. Running `docker-compose up` again will only make changes but won't recreate everything.

> **Difference between Dockerfile and Compose file:**
> A Dockerfile gives instructions for the **construction** of an image.
> A `compose.yaml` specifies **running containers** (services, networks, volumes).
> A compose file may **refer** to a Dockerfile.

#### Essential Compose commands:

```bash
# Start all services (build if needed, run in background)
docker compose up -d --build

# View running services
docker compose ps

# View logs
docker compose logs

# Stop and remove containers + networks
docker compose down

# Stop and also remove volumes
docker compose down --volumes
```

---

## 4. Key Docker Commands

| Command | Purpose |
|---|---|
| `docker run <image>` | Create and start a container from an image |
| `docker run -d` | Run container in detached (background) mode |
| `docker run -p 8080:80` | Map host port 8080 → container port 80 |
| `docker run -it` | Run interactively with a terminal |
| `docker ps` | List running containers |
| `docker ps -a` | List all containers (including stopped) |
| `docker stop <id>` | Stop a running container |
| `docker rm <id>` | Remove a stopped container |
| `docker pull <image>` | Download an image from a registry |
| `docker push <image>` | Upload an image to a registry |
| `docker build -t name .` | Build an image from a Dockerfile |
| `docker image ls` | List local images |
| `docker image rm <image>` | Remove a local image |
| `docker logs <id>` | View container logs |
| `docker exec -it <id> sh` | Open a shell inside a running container |

> **Tip:** You only need to provide enough of a container ID to make it unique. For example, `docker stop a1f` works if no other container ID starts with `a1f`.

---

## 5. Writing a Dockerfile

A `Dockerfile` is a text file with instructions to build a Docker image. Each instruction creates a new **layer**.

```dockerfile
# Start from an official base image
FROM node:18-alpine

# Set the working directory inside the container
WORKDIR /app

# Copy dependency files first (for better build cache use)
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY . .

# Expose the port the app runs on
EXPOSE 3000

# Command to run when the container starts
CMD ["node", "server.js"]
```

### Common Dockerfile Instructions

| Instruction | Purpose |
|---|---|
| `FROM` | Base image to start from |
| `WORKDIR` | Set working directory |
| `COPY` | Copy files from host into image |
| `RUN` | Execute a command during build |
| `EXPOSE` | Document which port the app uses |
| `CMD` | Default command to run when container starts |
| `ENV` | Set environment variables |
| `ARG` | Build-time variables |

---

## 6. Docker Compose File Example

```yaml
# compose.yaml
services:
  app:
    build: .               # Build image from Dockerfile in current directory
    ports:
      - "3000:3000"        # host:container port mapping
    environment:
      - DATABASE_URL=mysql://db:3306/mydb
    depends_on:
      - db                 # Start db before app

  db:
    image: mysql:8         # Use official MySQL image
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: mydb
    volumes:
      - db-data:/var/lib/mysql   # Persist database files

volumes:
  db-data:                 # Named volume declaration
```

---

> 📖 **Source:** [Docker Official Documentation — Get Started](https://docs.docker.com/get-started/)