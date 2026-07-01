# Docker Containers

## Overview

A Docker container is a lightweight, portable, and isolated runtime environment that packages an application along with all of its dependencies, libraries, configuration files, and runtime environment. Containers allow applications to run consistently across different environments, whether on a developer's laptop, a testing server, or a production system.

Unlike virtual machines, containers share the host operating system's kernel, making them significantly faster to start and more efficient in terms of CPU and memory usage.

---

# Image vs Container

| Docker Image                     | Docker Container                              |
| -------------------------------- | --------------------------------------------- |
| Read-only template               | Running instance of an image                  |
| Cannot be modified while running | Can be started, stopped, and removed          |
| Used to create containers        | Executes the application                      |
| Stored locally or in registries  | Exists only after being created from an image |

Think of it like this:

* **Image = Blueprint**
* **Container = Running House**

One image can be used to create multiple containers.

---

# Container Lifecycle

A Docker container moves through several states during its lifecycle.

```text
Image
   │
   ▼
Created
   │
   ▼
Running
   │
   ├────────► Paused
   │              │
   ▼              ▼
Stopped ◄─────────┘
   │
   ▼
Removed
```

### Created

The container has been created but has not started executing.

### Running

The application inside the container is actively executing.

### Paused

The container is temporarily suspended while preserving its current state.

### Stopped

The container has finished execution or has been manually stopped.

### Removed

The container is permanently deleted from the system.

---

# Why Containers Are Lightweight

Docker containers are lightweight because they do not include a full operating system. Instead, they share the host machine's operating system kernel.

Advantages include:

* Fast startup time
* Lower memory usage
* Reduced CPU overhead
* Better resource utilization
* Easy portability
* Consistent application behavior

Containers often start within seconds, making them ideal for DevOps workflows and cloud-native applications.

---

# Creating a Container

Pull an image:

```bash
docker pull nginx
```

Run a container:

```bash
docker run nginx
```

Run the container in detached mode:

```bash
docker run -d nginx
```

Assign a custom name:

```bash
docker run --name my-nginx nginx
```

Map a host port to the container:

```bash
docker run -d -p 8080:80 --name my-nginx nginx
```

Now open your browser and visit:

```
http://localhost:8080
```

You should see the default Nginx welcome page.

---

# Viewing Running Containers

List running containers:

```bash
docker ps
```

List all containers:

```bash
docker ps -a
```

---

# Inspecting a Container

View detailed information:

```bash
docker inspect my-nginx
```

This command displays:

* Container ID
* Network settings
* Mounted volumes
* Environment variables
* IP address
* Creation time

---

# Example Workflow

```bash
docker pull nginx

docker run -d --name my-nginx -p 8080:80 nginx

docker ps

docker inspect my-nginx

docker stop my-nginx

docker start my-nginx

docker rm -f my-nginx
```

This sequence demonstrates the complete lifecycle of a Docker container.

---

# Best Practices

* Use meaningful container names.
* Remove unused containers regularly.
* Run one primary application per container.
* Avoid running containers with unnecessary privileges.
* Keep containers lightweight by using minimal base images.

---

# Summary

Docker containers provide isolated environments for running applications quickly and consistently. Since containers share the host operating system's kernel, they consume fewer resources than traditional virtual machines while maintaining portability across different platforms.

Understanding containers is one of the most fundamental concepts in Docker and serves as the foundation for advanced topics such as Docker Compose, Kubernetes, and cloud-native application deployment.

---

