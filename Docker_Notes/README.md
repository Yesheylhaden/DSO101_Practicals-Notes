# DSO101_Practicals-Notes
# 🐳 Docker — Understanding Notes

---

## 1. Docker and its Purpose

Docker helps overcome the traditional **"it works on my machine"** issue through the packaging of your application and its requirements into a self-contained entity known as **container**.

> Traditional deployment approach – Many manual tasks that have potential issues and are hard to move from place to place.
> Docker deployment approach – Download an image -> Customize it -> Deploy ✔

---

## 2. Containers vs Virtual Machines

| Property | Virtual Machine | Container |
|---|---|---|
| Speed | Takes minutes | Quick to start up |
| Weight | Big (Full Operating System) | Light |
| Isolation | Each VM comes with its own full OS | Hosts share a Linux kernel |
| Efficiency | High overhead cost | Low overhead cost |

- Containers utilize the **Linux host's kernel**
- They are built using **Linux Namespaces** 
- Not VMs, but isolated processes

---

## 3. Basic Concepts of Docker

| Concept | Definition |
|---|---|
| **Docker engine** | Runtime environment installed in the host system for running containers |
| **Image** | Template from which a container can be instantiated |
| **Container** | Instantiation of an image |
| **Repository** | Service like Docker hub which hosts images |
| **Volume** | Persistent storage provided by Docker |
| **Bind mount** | Mounts a directory from the host system inside the container |

---

## 4. Docker CLI Commands

```bash
docker --version                            # Check Docker version
docker pull <image>                         # Download an image
docker run -d -p 80:80 --name web nginx     # Run container in background
docker ps                                   # List running containers
docker ps -a                                # List all containers
docker stop <name>                          # Stop a container
docker start <name>                         # Start a container
docker rm <name>                            # Remove a container
docker images                               # List local images
docker image list                           # List local images
docker rmi <image>                          # Remove an image
docker logs <name>                          # View container logs
docker exec -it <name> bash                 # Access container terminal
docker container list -a                    # List all containers (including stopped)
docker restart <name>                       # Restart a container
docker network list                         # List Docker networks
```

---

### Beginner Workflow Example
```bash
docker pull nginx                                   # Pull nginx image
docker run -d -p 8080:80 --name web-server nginx    # Run nginx container
docker ps                                           # Check running containers
docker logs web-server                              # View logs
docker stop web-server                              # Stop container
docker rm web-server                                # Remove container
```
---

## 5. Environment Variables

- The variables used to configure a container without entering it
- Used for configuring the timezone, username, and password
- The environment variables can be provided using an ad-hoc command, `docker-compose.yml`, or `.env` file

```bash
docker run -e TZ=Asia/Kolkata -e PUID=1000 my-image
```

---

## 6. Port Binding and Networking

- Port binding = **Destination NAT (DNAT)**
- Maps host port to container port

```bash
docker run -p 8080:80 nginx     # Host port 8080 ⇆ Container port 80
```

### Network Drivers

| Driver  | Purpose |
|--------|---------|
| **Bridge** | Default driver, allows communication between containers on the same host |
| **Host** | Uses host networking stack directly |
| **Overlay** | Multi-host networking for Docker Swarm |

> Containers residing on the same user-defined network can use their **container names** to access each other through Docker's integrated DNS service.

---

## 7. Dockerfile — The 5-Step Skeleton

A **Dockerfile** is a text file that gives instructions for building a Docker image.

```dockerfile
FROM python:3.9-slim         # 1. Base image (Foundation)
WORKDIR /app                 # 2. Set workdir (Setup)
COPY . .                     # 3. Copy source code into container (Transfer)
RUN pip install -r requirements.txt  # 4. Install dependencies (Assembly)
CMD ["python", "app.py"]     # 5. Execute upon launch (Operation)
```

| Instruction              | Meaning |
|--------------------------|---------|
| `FROM`                  | Base image |
| `WORKDIR`               | Workdir |
| `COPY`                  | Copies source code |
| `RUN`                   | Runs at **build time** |
| `CMD`                   | Runs at **image launch** |
| `EXPOSE`                | Documentation on which port your application runs |

> `.dockerignore` — ignores sensitive files (secrets, node_modules) from copying.

---

## 8. Layer Caching & Build Optimization

- Every `FROM`, `COPY`, `RUN` command results in a **cached layer**
- If nothing has been modified, Docker will reuse them → **increased build speed**

**Good Practice:** Place the most stable commands first

```dockerfile
# ✅ Good – dependencies get cached separately from the source code
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .

# ❌ Bad – code change requires running pip install again
COPY . .
RUN pip install -r requirements.txt
```

---

## 9. Docker Storage

| Storage Method | Description |
| -------------- | ----------- |
| **Volume**     | Managed persistent storage by Docker. Survives container removal |
| **Bind Mount** | Mount a host directory to container |

```bash
docker volume create my_data           # Creates a volume
docker run -v my_data:/var/lib/mysql mysql  # Uses volume in a container
docker volume prune                    # Removes unused volumes
```

> Containers are **disposable**. Always use volumes for storing any data. NEVER store data within a container.

---

## 10. Docker Compose

Use Docker Compose to create and manage **multiple containers at once** with just one file.

```yaml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "5000:5000"
  redis:
    image: "redis:alpine"
```

```bash
docker compose up -d   # Launches multiple containers as a service
docker compose down    # Kills and deletes multiple containers as a service
```

**Why Docker Compose?**
- Better readability and organization
- Creates its own network for services
- Easily replicable and version-controlled

---

## 11. Good Practices for Deploying Containers

- **Single responsibility per container** (separate containers for app and database)
- Do not keep any files on a container — use **volumes**
- Do not alter the container in`

## 12. Conclusion

Docker revolutionizes how software projects can be developed, distributed, and run. The use of containers makes sure that the code and its dependencies are isolated and consistent across different environments, making deployments easy and efficient.
Having a grasp of Docker components, such as images, containers, volumes, networks, and Dockerfiles, will allow for deploying applications effectively. Tools like Docker Compose make it easy to manage applications involving multiple containers.
The main takeaway regarding Docker's philosophy is as follows:

**Containers are disposable** – maintain them stateless
**Images are blueprints** – maintain them minimalistic and versioned
**Compose files are infrastructure** – maintain them in source control

From developers to system administrators and even network engineers, learning Docker should be on your list of skills for software deployment.