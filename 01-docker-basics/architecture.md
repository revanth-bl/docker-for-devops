# Docker Architecture

## Overview

Docker follows a client-server architecture that enables developers to build, ship, and run applications inside lightweight, portable containers. The Docker platform consists of several components working together to create, manage, and distribute containers across different environments.

Understanding Docker architecture is essential for DevOps engineers because it forms the foundation of modern application deployment, Continuous Integration/Continuous Deployment (CI/CD), cloud-native applications, and microservices.

---

# Learning Objectives

After completing this guide, you will be able to:

* Understand Docker architecture
* Explain the role of Docker Client and Docker Daemon
* Understand Docker Images, Containers, Registries, and Networks
* Describe the container lifecycle
* Understand how Docker processes commands
* Explain Docker architecture in technical interviews

---

# High-Level Architecture

```text
                   Docker Architecture

                 +--------------------+
                 |    Docker Client   |
                 |   (CLI / Desktop)  |
                 +---------+----------+
                           |
                    Docker REST API
                           |
                           ▼
                 +--------------------+
                 |   Docker Daemon    |
                 |     (dockerd)      |
                 +---------+----------+
                           |
        +------------------+------------------+
        |                  |                  |
        ▼                  ▼                  ▼
+---------------+  +----------------+  +--------------+
| Docker Images |  | Docker Network |  | Docker Volume|
+---------------+  +----------------+  +--------------+
        |
        ▼
+----------------------+
| Docker Containers    |
+----------------------+
        |
        ▼
+----------------------+
| Running Application  |
+----------------------+
```

---

# Core Components

## 1. Docker Client

The Docker Client is the interface used by users to communicate with Docker.

It accepts commands such as:

```bash
docker build

docker run

docker pull

docker push
```

The client sends requests to the Docker Daemon through the Docker REST API.

---

## 2. Docker Daemon (dockerd)

The Docker Daemon is the core service responsible for managing Docker resources.

Responsibilities include:

* Building images
* Running containers
* Managing networks
* Managing volumes
* Downloading images
* Removing unused resources

The daemon continuously listens for Docker API requests.

---

## 3. Docker Images

A Docker Image is a read-only template used to create containers.

Images contain:

* Application code
* Runtime
* Libraries
* Dependencies
* Configuration

Examples:

```text
ubuntu

nginx

mysql

node

python
```

Images are immutable.

Whenever changes are needed, a new image should be built.

---

## 4. Docker Containers

A container is a running instance of a Docker image.

Containers are:

* Lightweight
* Isolated
* Portable
* Fast
* Consistent

Multiple containers can be created from the same image.

Example:

```text
Image

     │

     ▼

Container 1

Container 2

Container 3
```

---

## 5. Docker Registry

A Docker Registry stores Docker images.

Popular registries include:

* Docker Hub
* GitHub Container Registry (GHCR)
* Amazon Elastic Container Registry (ECR)
* Google Artifact Registry
* Azure Container Registry

Workflow:

```text
Developer

     │

Build Image

     │

Push Image

     │

Docker Registry

     │

Pull Image

     │

Deploy Container
```

---

## 6. Docker Networks

Networks enable communication between containers.

Common network drivers:

* Bridge
* Host
* None
* Overlay
* Macvlan

Example:

```text
Web Container

       │

Bridge Network

       │

Database Container
```

---

## 7. Docker Volumes

Volumes provide persistent storage.

Without volumes:

```text
Container Deleted

↓

All Data Lost
```

With volumes:

```text
Container Deleted

↓

Volume Remains

↓

Data Preserved
```

Volumes are commonly used for:

* Databases
* Application logs
* User uploads
* Configuration files

---

# Docker Request Flow

When a user runs:

```bash
docker run nginx
```

Docker performs the following steps:

```text
User

 │

 ▼

Docker CLI

 │

 ▼

Docker Daemon

 │

 ▼

Check Local Image

 │

 ├── Found

 │      │

 │      ▼

 │  Create Container

 │

 └── Not Found

        │

        ▼

 Download Image

        │

        ▼

 Create Container

        │

        ▼

 Start Container
```

---

# Container Lifecycle

```text
Create

   │

   ▼

Start

   │

   ▼

Running

   │

   ├── Pause

   │

   ▼

Stop

   │

   ▼

Restart

   │

   ▼

Remove
```

---

# Docker Engine Architecture

```text
+--------------------------------------+
|          Docker Engine               |
+--------------------------------------+
|                                      |
| Docker Client                        |
| Docker REST API                      |
| Docker Daemon                        |
| Images                               |
| Containers                           |
| Networks                             |
| Volumes                              |
| Plugins                              |
| Logging                              |
+--------------------------------------+
```

---

# Real-World DevOps Workflow

Example:

A developer pushes application code to GitHub.

```text
GitHub

    │

GitHub Actions

    │

Build Docker Image

    │

Push to Docker Hub

    │

Kubernetes Pulls Image

    │

Deploy Application

    │

Users Access Service
```

Docker ensures the application behaves consistently across development, testing, staging, and production environments.

---

# Advantages of Docker Architecture

* Platform independent
* Lightweight containers
* Fast deployment
* Consistent environments
* Efficient resource utilization
* Easy scaling
* Supports CI/CD
* Ideal for microservices
* Simplifies cloud deployments

---

# Best Practices

* Keep images small.
* Use official base images.
* Remove unused images and containers.
* Store persistent data in volumes.
* Avoid running containers as the root user.
* Use Docker Compose for multi-container applications.
* Regularly update images to receive security patches.

---

# Common Mistakes

❌ Running multiple unrelated applications inside one container.

❌ Using the `latest` tag in production without version control.

❌ Storing database files inside the container filesystem.

❌ Ignoring image vulnerabilities.

❌ Building unnecessarily large Docker images.

---

# Troubleshooting

## Docker Daemon Not Running

Check Docker service:

```bash
docker info
```

Start Docker Desktop or restart the Docker service.

---

## Unable to Pull Image

Verify:

* Internet connection
* Image name
* Registry availability

Pull manually:

```bash
docker pull nginx
```

---

## Container Stops Immediately

Inspect logs:

```bash
docker logs <container-id>
```

---

## View Running Containers

```bash
docker ps
```

---

## View All Containers

```bash
docker ps -a
```

---

# Interview Questions

### What is Docker Architecture?

Docker Architecture is a client-server model consisting of the Docker Client, Docker Daemon, Docker Images, Containers, Networks, Volumes, and Registries that work together to build, distribute, and run containerized applications.

---

### What is Docker Engine?

Docker Engine is the core runtime that builds, manages, and runs Docker containers. It includes the Docker Daemon, REST API, and Docker CLI.

---

### What is the difference between an Image and a Container?

An Image is a read-only template containing an application's code, dependencies, and runtime.

A Container is a running instance of an image.

---

### Why are Docker Volumes important?

Volumes provide persistent storage independent of the container lifecycle, ensuring data is not lost when containers are removed.

---

### How does Docker communicate with the Daemon?

The Docker Client communicates with the Docker Daemon using the Docker REST API over a Unix socket or TCP connection.

---

# Summary

Docker architecture follows a client-server model designed for portability, scalability, and consistency. By separating the client, daemon, images, containers, registries, networks, and volumes, Docker enables modern DevOps teams to build, ship, and deploy applications efficiently across multiple environments.

A solid understanding of Docker architecture is essential before learning Dockerfiles, Docker Compose, Kubernetes, and container orchestration.

---

# References

Official Docker Documentation

https://docs.docker.com/

Docker Engine Overview

https://docs.docker.com/engine/

Docker Architecture

https://docs.docker.com/get-started/docker-overview/
