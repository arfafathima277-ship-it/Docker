# Docker Images

## 1. What is a Docker Image?

A Docker image is a **read-only, immutable template** used to create Docker containers.

It contains everything required to run an application:

* Application code
* Runtime
* Libraries
* Dependencies
* Configuration files
* System tools

```text
Docker Image
      |
      +----> Container 1
      |
      +----> Container 2
      |
      +----> Container 3
```

**One image can create multiple containers.**

---

## 2. Characteristics of Docker Images

* Read-only
* Immutable
* Lightweight
* Portable
* Reusable
* Versionable
* Made up of multiple layers
* Can be stored in container registries

---

## 3. Docker Image Layers

Docker images are made up of multiple **read-only layers**.

Example Dockerfile:

```dockerfile
FROM ubuntu
RUN apt update
RUN apt install -y nginx
COPY index.html /var/www/html/
```

Conceptually:

```text
Layer 4 → COPY index.html
Layer 3 → Install nginx
Layer 2 → apt update
Layer 1 → Ubuntu base image
```

### Advantages of Layers

* Faster builds
* Layer caching
* Reusability
* Reduced storage
* Faster image transfer

---

## 4. Docker Image vs Container

| Image                            | Container                |
| -------------------------------- | ------------------------ |
| Read-only template               | Instance of an image     |
| Immutable                        | Has a writable layer     |
| Used to create containers        | Runs the application     |
| Contains application environment | Contains running process |

```text
Image → Container → Application
```

---

## 5. Dockerfile and Image

A **Dockerfile** contains instructions used to build a Docker image.

Example:

```dockerfile
FROM nginx:latest

COPY index.html /usr/share/nginx/html/

EXPOSE 80
```

Build the image:

```bash
docker build -t my-nginx:v1 .
```

---

## 6. Docker Image Naming

General format:

```text
repository/image:tag
```

Example:

```text
myapp:v1
nginx:latest
```

With a registry:

```text
docker.io/username/myapp:v1
```

### Components

```text
docker.io / username / myapp : v1
    |          |         |      |
 Registry   User      Image    Tag
```

---

## 7. Docker Image Tags

A tag identifies a version or variant of an image.

Examples:

```text
myapp:v1
myapp:v2
myapp:1.0
myapp:production
myapp:latest
```

**Important:** `latest` is only a tag name. It does not always mean the newest image.

---

## 8. Build an Image

Create a file named:

```text
Dockerfile
```

Then:

```bash
docker build -t myapp:v1 .
```

Explanation:

* `docker build` → Builds an image
* `-t` → Assigns name and tag
* `myapp:v1` → Image name and version
* `.` → Build context

---

## 9. List Images

```bash
docker images
```

or:

```bash
docker image ls
```

Example:

```text
REPOSITORY   TAG       IMAGE ID       SIZE
nginx        latest    abc123         192MB
ubuntu       latest    def456         78MB
```

---

## 10. Pull an Image

Download an existing image from a registry:

```bash
docker pull nginx
```

Specific version:

```bash
docker pull nginx:1.27
```

---

## 11. Run a Container from an Image

```bash
docker run nginx
```

Docker uses the image to create and start a container.

```text
Docker Image
     ↓
docker run
     ↓
Docker Container
```

---

## 12. Tag an Image

Tag an existing image:

```bash
docker tag myapp:v1 username/myapp:v1
```

This is commonly done before pushing an image to Docker Hub or another registry.

---

## 13. Push an Image

Push an image to a container registry:

```bash
docker push username/myapp:v1
```

Workflow:

```text
Local Image
    ↓
docker tag
    ↓
docker push
    ↓
Container Registry
```

---

## 14. Docker Image Registry

A container registry stores and distributes Docker images.

Examples:

* Docker Hub
* Amazon ECR
* GitHub Container Registry
* Azure Container Registry
* Google Artifact Registry

---

## 15. Inspect an Image

To view detailed information:

```bash
docker inspect myapp:v1
```

---

## 16. View Image History

To view image layers and build history:

```bash
docker history myapp:v1
```

---

## 17. Remove an Image

```bash
docker rmi myapp:v1
```

or:

```bash
docker image rm myapp:v1
```

---

## 18. Remove Unused Images

Remove dangling images:

```bash
docker image prune
```

Remove unused images more broadly:

```bash
docker image prune -a
```

---

## 19. Docker Image Cache

Docker uses cached layers during image builds.

If a layer has not changed, Docker can reuse it instead of rebuilding it.

This results in:

* Faster builds
* Less processing
* Efficient development

---

## 20. `.dockerignore`

`.dockerignore` prevents unnecessary files from being sent as part of the build context.

Example:

```text
.git
node_modules
*.log
.env
```

Benefits:

* Smaller build context
* Faster builds
* Avoids unnecessary files
* Helps prevent sensitive files from being included

---

## 21. Image Digest

A Docker image can also be identified using a digest.

Example:

```text
nginx@sha256:abc123...
```

A digest identifies specific image content.

### Tag vs Digest

```text
Tag:
nginx:latest

Digest:
nginx@sha256:...
```

Tags can point to different image versions over time, while a digest identifies specific content.

---

## 22. Reducing Docker Image Size

Best practices:

* Use an appropriate base image
* Use `.dockerignore`
* Remove unnecessary dependencies
* Copy only required files
* Use multi-stage builds
* Keep the image focused on one application

Example:

```dockerfile
FROM python:3.12-slim
```

A slim base image can reduce unnecessary packages compared with a larger general-purpose image.

---

## 23. Docker Image Lifecycle

```text
Dockerfile
    ↓
docker build
    ↓
Docker Image
    ↓
docker tag
    ↓
docker push
    ↓
Container Registry
    ↓
docker pull
    ↓
Docker Image
    ↓
docker run
    ↓
Container
```

---

## 24. Important Docker Image Commands

```bash
# List images
docker images

# Pull image
docker pull nginx

# Build image
docker build -t myapp:v1 .

# Run container
docker run myapp:v1

# Tag image
docker tag myapp:v1 username/myapp:v1

# Push image
docker push username/myapp:v1

# Inspect image
docker inspect myapp:v1

# View image history
docker history myapp:v1

# Remove image
docker rmi myapp:v1

# Remove unused images
docker image prune
```

---

# Interview Questions

### Q1. What is a Docker image?

A Docker image is a read-only, immutable template used to create Docker containers.

### Q2. Can one image create multiple containers?

Yes. Multiple containers can be created from the same image.

### Q3. Are Docker images mutable?

No. Docker images are immutable. Changes are normally made by creating a new image version.

### Q4. What are Docker image layers?

Docker image layers are read-only filesystem layers that together form a Docker image.

### Q5. Why does Docker use layers?

Layers provide caching, reusability, faster builds, and efficient storage.

### Q6. What is the difference between `docker build` and `docker pull`?

`docker build` creates an image using a Dockerfile.

`docker pull` downloads an existing image from a registry.

### Q7. What is the difference between an image and a container?

An image is a read-only template, while a container is an instance created from that image.

### Q8. What is `latest`?

`latest` is a Docker image tag. It does not necessarily mean the newest image.

### Q9. How do you check image layers?

```bash
docker history <image>
```

### Q10. How do you remove a Docker image?

```bash
docker rmi <image>
```

---

# Key Takeaways

```text
Dockerfile → docker build → Image
Image → docker run → Container

Image = Read-only template
Container = Running instance

Image = Immutable
Image = Multiple layers
Image = Reusable

docker build  → Create image
docker pull   → Download image
docker push   → Upload image
docker run    → Create/start container
docker tag    → Tag image
docker history → View layers
docker inspect → View details
docker rmi     → Remove image
```
