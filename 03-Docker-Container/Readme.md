# Docker Containers

## 1. What is a Docker Container?

A Docker container is a **lightweight, isolated, runnable instance of a Docker image**.

A container contains:

* Application
* Dependencies
* Configuration
* Runtime environment
* Writable container layer

### Simple Definition

> **Docker Image = Template**
> **Docker Container = Running instance of that image**

```text
Docker Image
      |
      | docker run
      ↓
Docker Container
      |
      ↓
Application
```

---

## 2. How is a Container Created?

A container is created from a Docker image.

Example:

```bash
docker run nginx
```

Docker uses the `nginx` image to create and start a container.

```text
nginx Image
     ↓
docker run
     ↓
nginx Container
```

---

## 3. Docker Container Characteristics

Docker containers are:

* Lightweight
* Isolated
* Portable
* Fast to start
* Ephemeral by default
* Based on Docker images
* Isolated from other containers
* Able to communicate through Docker networks
* Able to store persistent data using volumes

---

## 4. Container Lifecycle

A container can have different states.

```text
Created
   ↓
Running
   ↓
Stopped
   ↓
Removed
```

A common lifecycle:

```text
Image
  ↓
docker create
  ↓
Created
  ↓
docker start
  ↓
Running
  ↓
docker stop
  ↓
Stopped
  ↓
docker rm
  ↓
Removed
```

---

## 5. Create a Container

To create a container without starting it:

```bash
docker create nginx
```

The container will be created in the `Created` state.

---

## 6. Start a Container

Start an existing container:

```bash
docker start <container>
```

Example:

```bash
docker start my-nginx
```

---

## 7. Create and Start a Container

The most commonly used command is:

```bash
docker run nginx
```

`docker run` generally creates a new container from the image and starts it.

---

## 8. List Running Containers

```bash
docker ps
```

Example:

```text
CONTAINER ID   IMAGE   STATUS       PORTS
abc123         nginx   Up 2 minutes 80/tcp
```

---

## 9. List All Containers

To display both running and stopped containers:

```bash
docker ps -a
```

This is very important.

```text
docker ps     → Running containers
docker ps -a  → All containers
```

---

## 10. Run Container in Background

Use detached mode:

```bash
docker run -d nginx
```

`-d` means **detached mode**.

The container runs in the background and the terminal remains available.

---

## 11. Assign a Name to a Container

Docker can automatically generate a container name.

You can also provide your own name:

```bash
docker run -d --name my-nginx nginx
```

Now the container can be referenced using:

```bash
docker start my-nginx
docker stop my-nginx
docker logs my-nginx
```

---

## 12. Stop a Container

```bash
docker stop my-nginx
```

This gracefully stops the running container.

---

## 13. Kill a Container

```bash
docker kill my-nginx
```

`docker kill` immediately sends a kill signal to the container.

### Difference

```text
docker stop → Graceful stop
docker kill → Forceful/immediate stop
```

---

## 14. Restart a Container

```bash
docker restart my-nginx
```

This stops and starts the container again.

---

## 15. Remove a Container

```bash
docker rm my-nginx
```

A running container normally needs to be stopped before removal.

You can force removal using:

```bash
docker rm -f my-nginx
```

---

## 16. Container Logs

To view container logs:

```bash
docker logs my-nginx
```

Follow logs continuously:

```bash
docker logs -f my-nginx
```

`-f` means follow the log output.

---

## 17. Execute Commands Inside a Container

Use:

```bash
docker exec
```

Example:

```bash
docker exec -it my-nginx /bin/bash
```

For images that don't contain Bash:

```bash
docker exec -it my-nginx /bin/sh
```

### Explanation

```text
-i → Interactive
-t → Allocate terminal
```

---

## 18. Access a Running Container

Example:

```bash
docker exec -it my-container /bin/bash
```

This gives an interactive shell inside the running container.

Important:

> `docker exec` runs a new process inside an already running container.

---

## 19. Container Port Mapping

Containers have their own network namespace.

If an application inside a container listens on port `80`, it does not automatically mean the application is accessible through port `80` on the host.

Use:

```bash
docker run -d -p 8080:80 nginx
```

Format:

```text
-p HOST_PORT:CONTAINER_PORT
```

Example:

```text
Host Port 8080
      ↓
Container Port 80
```

You can access the application through:

```text
localhost:8080
```

---

## 20. Container Environment Variables

Environment variables can be passed using `-e`.

Example:

```bash
docker run -d -e APP_ENV=production myapp
```

Multiple variables:

```bash
docker run -d \
  -e APP_ENV=production \
  -e APP_PORT=8080 \
  myapp
```

---

## 21. Container Volumes

Containers are generally ephemeral.

If data needs to survive container deletion, use a volume.

Example:

```bash
docker volume create mydata
```

Run a container with the volume:

```bash
docker run -d \
  --name myapp \
  -v mydata:/data \
  myapp
```

```text
Container
    |
    ↓
/data
    |
    ↓
Docker Volume
```

---

## 22. Why Volumes Are Important

Without persistent storage:

```text
Container
   ↓
Container Deleted
   ↓
Container Writable Data Lost
```

With a volume:

```text
Container
   ↓
Volume
   ↓
Container Deleted
   ↓
Volume Data Remains
```

Volumes are commonly used for:

* Databases
* Application data
* Uploaded files
* Persistent configuration/data

---

## 23. Container Networking

Containers can communicate with each other through Docker networks.

Create a network:

```bash
docker network create mynetwork
```

Run containers on the network:

```bash
docker run -d --name app --network mynetwork myapp
```

```bash
docker run -d --name db --network mynetwork mysql
```

Containers on the same user-defined network can communicate using container/service names.

Example:

```text
app → db
```

---

## 24. Container Isolation

Each container has an isolated environment.

Containers can have their own:

* Filesystem
* Processes
* Network
* Environment variables
* Resource limits

However, containers share the host operating system kernel.

---

## 25. Containers vs Virtual Machines

| Containers                | Virtual Machines                   |
| ------------------------- | ---------------------------------- |
| Lightweight               | Heavier                            |
| Share host kernel         | Have a guest OS                    |
| Start quickly             | Usually slower to start            |
| Use fewer resources       | Use more resources                 |
| Container isolation       | Hardware/VM-level virtualization   |
| Designed for applications | Can run complete operating systems |

---

## 26. Container Writable Layer

Docker images are read-only.

When a container is created, Docker adds a writable layer on top of the image.

```text
+--------------------------+
| Container Writable Layer |
+--------------------------+
| Image Layer 3            |
+--------------------------+
| Image Layer 2            |
+--------------------------+
| Image Layer 1            |
+--------------------------+
```

Changes made inside the container are stored in this writable layer unless they are stored in a volume or another persistent storage mechanism.

---

## 27. Ephemeral Containers

Containers are generally considered **ephemeral**.

This means containers can be created, stopped, deleted, and recreated easily.

Applications should ideally keep important persistent data outside the container's writable layer.

For example:

```text
Application Container
        |
        ↓
Persistent Volume
```

---

## 28. Container Resource Limits

Docker allows CPU and memory resources to be limited.

Example:

```bash
docker run -d --memory="512m" nginx
```

Limit CPU:

```bash
docker run -d --cpus="1.0" nginx
```

This prevents a container from consuming unlimited resources.

---

## 29. Container Health Check

A health check allows Docker to determine whether an application inside a container is healthy.

Example Dockerfile:

```dockerfile
HEALTHCHECK CMD curl --fail http://localhost:8080/ || exit 1
```

Docker can report the container health status as:

```text
starting
healthy
unhealthy
```

---

## 30. Container Restart Policies

Docker supports restart policies.

Example:

```bash
docker run -d --restart=always nginx
```

Common restart policies:

```text
no
always
on-failure
unless-stopped
```

Example:

```bash
docker run -d --restart=unless-stopped nginx
```

---

## 31. Copy Files Between Host and Container

Copy from host to container:

```bash
docker cp index.html my-nginx:/usr/share/nginx/html/
```

Copy from container to host:

```bash
docker cp my-nginx:/etc/nginx/nginx.conf .
```

---

## 32. Inspect a Container

```bash
docker inspect my-nginx
```

This provides detailed information such as:

* Container configuration
* Network information
* Mounts
* Environment variables
* IP address
* State
* Restart policy

---

## 33. Container Statistics

To monitor resource usage:

```bash
docker stats
```

It displays information such as:

* CPU usage
* Memory usage
* Network I/O
* Block I/O

For a specific container:

```bash
docker stats my-nginx
```

---

## 34. Container Processes

To see processes running inside a container:

```bash
docker top my-nginx
```

---

## 35. Pause and Unpause a Container

Pause processes inside a container:

```bash
docker pause my-nginx
```

Resume:

```bash
docker unpause my-nginx
```

---

## 36. Rename a Container

```bash
docker rename old-name new-name
```

Example:

```bash
docker rename nginx-container web-server
```

---

# Important Docker Container Commands

```bash
# List running containers
docker ps

# List all containers
docker ps -a

# Create container
docker create nginx

# Start container
docker start <container>

# Run container
docker run nginx

# Run in background
docker run -d nginx

# Give container a name
docker run -d --name my-nginx nginx

# Stop container
docker stop <container>

# Kill container
docker kill <container>

# Restart container
docker restart <container>

# Remove container
docker rm <container>

# Force remove container
docker rm -f <container>

# View logs
docker logs <container>

# Follow logs
docker logs -f <container>

# Execute command inside container
docker exec -it <container> /bin/bash

# Inspect container
docker inspect <container>

# Monitor resources
docker stats

# View processes
docker top <container>

# Copy files
docker cp <source> <destination>

# Pause container
docker pause <container>

# Resume container
docker unpause <container>
```

# Docker Container Lifecycle

```text
                 Docker Image
                      |
                      ↓
                docker create
                      |
                      ↓
                   Created
                      |
                      ↓
                docker start
                      |
                      ↓
                   Running
                  /       \
                 /         \
        docker stop      docker restart
              |               |
              ↓               ↓
           Stopped ←------ Running
              |
              ↓
          docker rm
              |
              ↓
           Removed
```

# Image → Container Flow

```text
Dockerfile
    ↓
docker build
    ↓
Docker Image
    ↓
docker run
    ↓
Docker Container
    ↓
Application
```

# Interview Questions

### Q1. What is a Docker container?

A Docker container is an isolated, runnable instance of a Docker image.

### Q2. What is the difference between an image and a container?

An image is a read-only template, while a container is an instance created from that image with a writable layer.

### Q3. What is the difference between `docker run` and `docker start`?

`docker run` creates a new container from an image and starts it.

`docker start` starts an existing stopped container.

### Q4. What is the difference between `docker create` and `docker run`?

`docker create` creates a container but does not start it.

`docker run` creates and starts a container.

### Q5. What is the difference between `docker stop` and `docker kill`?

`docker stop` attempts a graceful shutdown.

`docker kill` immediately terminates the container process.

### Q6. What is `docker exec`?

`docker exec` runs a new command/process inside an already running container.

### Q7. How do you list all containers?

```bash
docker ps -a
```

### Q8. How do you access a running container?

```bash
docker exec -it <container> /bin/bash
```

or:

```bash
docker exec -it <container> /bin/sh
```

### Q9. How do you expose a container port?

Use port mapping:

```bash
docker run -d -p 8080:80 nginx
```

### Q10. Why do we use Docker volumes?

Volumes are used to persist data independently of the container lifecycle.

### Q11. What happens to data stored only in the container writable layer?

It is lost when the container is removed.

### Q12. Can multiple containers use the same Docker image?

Yes.

### Q13. Can multiple containers use the same volume?

Yes, depending on the application's requirements and access pattern.

### Q14. What is a container restart policy?

A restart policy determines when Docker should automatically restart a container.

Example:

```bash
docker run -d --restart=always nginx
```

### Q15. How do you check container logs?

```bash
docker logs <container>
```

### Q16. How do you check container resource usage?

```bash
docker stats
```

# Key Takeaways

```text
Image       → Read-only template
Container   → Instance of image

docker run    → Create + Start
docker create → Create only
docker start  → Start existing container

docker ps     → Running containers
docker ps -a  → All containers

docker stop   → Graceful stop
docker kill   → Immediate stop

docker rm     → Remove container
docker exec   → Execute command inside container
docker logs   → View logs
docker stats  → Resource usage

Volume        → Persistent data
Network       → Container communication
Port mapping  → Host ↔ Container access
```

## Most Important Concept

> **Containers are designed to be disposable. Store important data in persistent storage such as Docker volumes rather than relying on the container's writable layer.**
