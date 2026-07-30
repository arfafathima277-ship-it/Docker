# Docker Basics

## What is Docker?

Docker is an open-source containerization platform that packages an application along with its dependencies, libraries, and configuration files into a container. This ensures the application runs consistently across different environments.

### Key Points
- Open-source platform
- Uses containerization
- Packages applications with dependencies
- Ensures consistency across environments
- Lightweight and portable

---

## Why Docker?

Before Docker, developers had to manually install dependencies and software versions on different servers. This often caused deployment failures due to missing libraries or version mismatches.

Docker solves this problem by packaging everything the application needs into a container.

### Advantages
- Eliminates dependency issues
- Faster deployment
- Consistent environments
- Lightweight
- Easy scalability
- Better resource utilization
- Application isolation

---

## Docker vs Virtual Machine

| Docker | Virtual Machine |
|--------|-----------------|
| Uses containers | Uses virtualization |
| Lightweight | Heavyweight |
| Uses MBs of storage | Uses GBs of storage |
| Starts within seconds | Takes longer to boot |
| Shares the host OS kernel | Has its own guest OS and kernel |
| Uses less CPU and RAM | Uses more CPU and RAM |
| Best for application deployment | Best for complete OS isolation |

### Why is Docker Faster?

Docker shares the host operating system's kernel instead of running a separate guest operating system. This reduces startup time and resource usage.

---

# Docker Architecture

Docker Architecture consists of the following components:

- Docker Client
- Docker API
- Docker Daemon
- Docker Registry (Docker Hub)

### Architecture Flow

User
↓
Docker Client
↓
Docker API
↓
Docker Daemon
↓
Docker Hub (If image is not available locally)
↓
Docker Image
↓
Docker Container

---

## Docker Client

Docker Client is the command-line interface (CLI) used to interact with Docker.

It accepts commands from the user and sends them to the Docker Daemon through the Docker API.

### Example Commands

```bash
docker run nginx
docker images
docker ps
docker stop <container_id>
```

---

## Docker API

Docker API is the communication layer between the Docker Client and the Docker Daemon.

It receives requests from the Docker Client and forwards them to the Docker Daemon. After processing, the response is sent back to the Docker Client.

---

## Docker Daemon

Docker Daemon (`dockerd`) is the core service of Docker.

It is responsible for managing Docker objects such as:
- Images
- Containers
- Networks
- Volumes

### Responsibilities

- Build images
- Pull images
- Push images
- Create containers
- Start containers
- Stop containers
- Remove containers
- Manage networks
- Manage volumes

---

## Docker Registry

Docker Registry is a repository used to store Docker Images.

A registry can be:
- Public
- Private

### Popular Registry

Docker Hub is the default public Docker Registry where users can pull and push Docker Images.

Examples of official images:
- Ubuntu
- Nginx
- Redis
- MySQL
- Jenkins
- Python
- Node.js

---

## Docker Image

A Docker Image is a read-only template used to create Docker Containers.

It contains:
- Application
- Dependencies
- Libraries
- Configuration files
- Runtime Environment

One Docker Image can create multiple Docker Containers.

---

## Image vs Container

| Docker Image | Docker Container |
|--------------|------------------|
| Read-only template | Running instance of an image |
| Cannot execute by itself | Executes the application |
| Used to create containers | Created from images |

---

## Docker Image Lifecycle

Docker Hub
↓
docker pull nginx
↓
Docker Image
↓
docker run nginx
↓
Docker Container

---

## Common Docker Commands

### Download an image

```bash
docker pull nginx
```

### List all images

```bash
docker images
```

### Run a container

```bash
docker run nginx
```

### Remove an image

```bash
docker rmi nginx
```

### Inspect an image

```bash
docker inspect nginx
```

---

## Difference Between docker pull and docker run

### docker pull

- Downloads the image from Docker Hub.
- Does not create a container.

### docker run

- Checks if the image exists locally.
- Downloads it if not available.
- Creates a container.
- Starts the container.

---

# Interview Questions

1. What is Docker?
2. Why is Docker used?
3. Docker vs Virtual Machine.
4. What is Docker Architecture?
5. What is Docker Client?
6. What is Docker API?
7. What is Docker Daemon?
8. What is Docker Registry?
9. What is Docker Hub?
10. What is a Docker Image?
11. Difference between Image and Container.
12. Difference between docker pull and docker run.
13. What happens internally when you run `docker run nginx`?

---

# Summary

- Docker is a containerization platform.
- Docker packages applications with dependencies.
- Containers share the host operating system's kernel.
- Docker Engine consists of Docker Client, Docker API, and Docker Daemon.
- Docker Hub is the default public Docker Registry.
- Images are templates used to create containers.
- `docker pull` downloads an image.
- `docker run` creates and starts a container.
