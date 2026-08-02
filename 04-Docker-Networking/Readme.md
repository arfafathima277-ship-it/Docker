# Docker Networking

## What is Docker Networking?

Docker Networking enables communication between Docker containers, the Docker host, and external networks. It allows containers to exchange data securely and efficiently.

---

# Why Do We Need Docker Networking?

- Communication between containers
- Communication with external applications
- Isolation between applications
- Secure network connectivity
- Support for microservices

---

# Types of Docker Networks

## 1. Bridge Network

- Default network created by Docker.
- Allows communication between containers on the same host.
- Suitable for standalone containers.

---

## 2. Host Network

- Shares the host machine's network.
- No network isolation.
- Provides better performance.

---

## 3. None Network

- No network access.
- Container is completely isolated.

---

## 4. Overlay Network

- Connects containers across multiple Docker hosts.
- Commonly used with Docker Swarm.

---

## 5. Macvlan Network

- Assigns a unique MAC address to each container.
- Makes containers appear as physical devices on the network.

---

# Docker Networking Diagram

```text
                 Docker Host
          ┌────────────────────┐
          │    Bridge Network  │
          │                    │
          │ ┌───────────────┐  │
          │ │ Container A   │◄─┐
          │ └───────────────┘  │
          │         ▲          │
          │         │          │
          │ ┌───────────────┐  │
          │ │ Container B   │──┘
          │ └───────────────┘  │
          └────────────────────┘
```

---

# Common Docker Network Commands

## List Networks

```bash
docker network ls
```

---

## Create a Network

```bash
docker network create mynetwork
```

---

## Inspect a Network

```bash
docker network inspect mynetwork
```

---

## Connect a Container

```bash
docker network connect mynetwork container-name
```

---

## Disconnect a Container

```bash
docker network disconnect mynetwork container-name
```

---

## Remove a Network

```bash
docker network rm mynetwork
```

---

# Advantages

- Secure communication
- Container isolation
- Easy service discovery
- Supports microservices
- Improved scalability

---

# Real-Time Example

A web application consists of:

- Nginx (Frontend)
- Node.js (Backend)
- MySQL (Database)

All three containers communicate over the same Docker bridge network without exposing the database to the internet.

---

# Interview Questions

### 1. What is Docker Networking?

Docker Networking allows containers to communicate with each other, the host, and external systems.

---

### 2. What is the default Docker network?

Bridge Network.

---

### 3. Which network mode provides no isolation?

Host Network.

---

### 4. Which network completely isolates a container?

None Network.

---

### 5. Which network is used in Docker Swarm?

Overlay Network.

---

### 6. Which command lists Docker networks?

```bash
docker network ls
```

---

### 7. Which command creates a Docker network?

```bash
docker network create mynetwork
```

---

# Summary

- Docker Networking enables communication between containers.
- Bridge is the default network.
- Host shares the host's network.
- None provides complete isolation.
- Overlay connects containers across multiple hosts.