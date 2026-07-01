# Docker Basic Commands

## Overview

Docker provides a powerful Command Line Interface (CLI) for managing images, containers, networks, volumes, and system resources. Mastering these commands is essential for DevOps engineers, cloud engineers, and software developers working with containerized applications.

This guide covers the most frequently used Docker commands with syntax, examples, best practices, troubleshooting tips, and real-world DevOps use cases.

---

# Learning Objectives

After completing this guide, you will be able to:

* Verify Docker installation
* Manage Docker images
* Create and manage containers
* View logs and system information
* Work with Docker networks and volumes
* Clean up Docker resources
* Apply Docker commands in DevOps workflows

---

# Command Categories

```text
Docker Commands
│
├── Information
├── Images
├── Containers
├── Networks
├── Volumes
├── Logs
├── System Cleanup
└── Registry
```

---

# Docker Information Commands

## Check Docker Version

```bash
docker --version
```

Example Output:

```text
Docker version 28.x.x
```

---

## Display Docker Information

```bash
docker info
```

Displays:

* Docker Engine version
* Number of containers
* Images
* Storage driver
* CPU
* Memory
* Docker Root Directory

---

## Display Docker Help

```bash
docker --help
```

Display help for a specific command:

```bash
docker run --help
```

---

# Image Commands

## List Images

```bash
docker images
```

---

## Pull an Image

```bash
docker pull nginx
```

---

## Build an Image

```bash
docker build -t myapp:v1 .
```

---

## Remove an Image

```bash
docker rmi nginx
```

---

## Inspect an Image

```bash
docker inspect nginx
```

---

# Container Commands

## Create and Run a Container

```bash
docker run nginx
```

---

## Run a Container in Detached Mode

```bash
docker run -d nginx
```

---

## Run with Port Mapping

```bash
docker run -d -p 8080:80 nginx
```

---

## Assign a Container Name

```bash
docker run --name web-server nginx
```

---

## List Running Containers

```bash
docker ps
```

---

## List All Containers

```bash
docker ps -a
```

---

## Stop a Container

```bash
docker stop <container-id>
```

---

## Start a Container

```bash
docker start <container-id>
```

---

## Restart a Container

```bash
docker restart <container-id>
```

---

## Remove a Container

```bash
docker rm <container-id>
```

---

## Execute a Command Inside a Container

```bash
docker exec -it <container-id> bash
```

---

## View Container Logs

```bash
docker logs <container-id>
```

---

# Network Commands

## List Networks

```bash
docker network ls
```

---

## Create a Network

```bash
docker network create app-network
```

---

## Inspect a Network

```bash
docker network inspect app-network
```

---

## Remove a Network

```bash
docker network rm app-network
```

---

# Volume Commands

## List Volumes

```bash
docker volume ls
```

---

## Create a Volume

```bash
docker volume create app-data
```

---

## Inspect a Volume

```bash
docker volume inspect app-data
```

---

## Remove a Volume

```bash
docker volume rm app-data
```

---

# Registry Commands

## Login to Docker Hub

```bash
docker login
```

---

## Push an Image

```bash
docker push username/myapp:v1
```

---

## Pull an Image

```bash
docker pull username/myapp:v1
```

---

## Logout

```bash
docker logout
```

---

# System Commands

## View Docker Disk Usage

```bash
docker system df
```

---

## Remove Unused Resources

```bash
docker system prune
```

---

## Remove Everything Unused

```bash
docker system prune -a
```

---

## Remove Stopped Containers

```bash
docker container prune
```

---

## Remove Unused Images

```bash
docker image prune
```

---

## Remove Unused Volumes

```bash
docker volume prune
```

---

# Real-World DevOps Workflow

A developer builds and deploys a web application using Docker.

```text
Source Code
      │
      ▼
docker build
      │
      ▼
Docker Image
      │
      ▼
docker run
      │
      ▼
Running Container
      │
      ▼
docker push
      │
      ▼
Docker Registry
      │
      ▼
CI/CD Pipeline
      │
      ▼
Production Deployment
```

---

# Frequently Used Commands

| Command               | Purpose                             |
| --------------------- | ----------------------------------- |
| `docker --version`    | Check Docker version                |
| `docker info`         | Display Docker information          |
| `docker images`       | List images                         |
| `docker pull`         | Download an image                   |
| `docker build`        | Build an image                      |
| `docker run`          | Create and run a container          |
| `docker ps`           | List running containers             |
| `docker ps -a`        | List all containers                 |
| `docker stop`         | Stop a container                    |
| `docker start`        | Start a container                   |
| `docker restart`      | Restart a container                 |
| `docker rm`           | Remove a container                  |
| `docker rmi`          | Remove an image                     |
| `docker exec`         | Execute commands inside a container |
| `docker logs`         | Display container logs              |
| `docker network ls`   | List networks                       |
| `docker volume ls`    | List volumes                        |
| `docker system prune` | Clean up unused resources           |

---

# Best Practices

* Use official Docker images whenever possible.
* Assign meaningful names to containers.
* Tag images with version numbers instead of using `latest`.
* Remove unused containers and images regularly.
* Store persistent data in Docker volumes.
* Review container logs when troubleshooting.
* Avoid running containers with unnecessary privileges.

---

# Common Mistakes

❌ Forgetting to map ports.

❌ Using the `latest` tag in production.

❌ Leaving stopped containers on the system.

❌ Ignoring Docker logs.

❌ Storing persistent data inside containers.

---

# Troubleshooting

## Docker Command Not Found

Verify Docker installation:

```bash
docker --version
```

---

## Cannot Connect to Docker Daemon

Check Docker service:

```bash
docker info
```

Start Docker Desktop or restart the Docker service.

---

## Container Stops Immediately

Inspect logs:

```bash
docker logs <container-id>
```

---

## Port Already in Use

Identify the process using the port or map the container to a different host port.

Example:

```bash
docker run -d -p 8081:80 nginx
```

---

# Interview Questions

### What command lists running containers?

```bash
docker ps
```

---

### What is the difference between `docker run` and `docker start`?

* `docker run` creates and starts a new container.
* `docker start` starts an existing stopped container.

---

### How do you execute a command inside a running container?

```bash
docker exec -it <container-id> bash
```

---

### Which command displays container logs?

```bash
docker logs <container-id>
```

---

### What does `docker system prune` do?

It removes unused containers, networks, images, and build cache to free disk space.

---

# Summary

Docker's CLI is the primary interface for interacting with Docker Engine. Understanding these commands enables developers and DevOps engineers to build, manage, troubleshoot, and deploy containerized applications efficiently. These commands also form the basis for Docker Compose, Kubernetes, and CI/CD automation.

---

# References

Official Docker Documentation

https://docs.docker.com/

Docker CLI Reference

https://docs.docker.com/reference/cli/

Docker Command Line Reference

https://docs.docker.com/engine/reference/commandline/
