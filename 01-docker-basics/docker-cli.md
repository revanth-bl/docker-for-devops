# Docker CLI (Command Line Interface)

## Overview

The Docker Command Line Interface (CLI) is the primary tool used to interact with Docker Engine. It enables developers and DevOps engineers to build images, manage containers, configure networks, handle storage, and interact with container registries directly from the terminal.

The Docker CLI communicates with the Docker Daemon (`dockerd`) through the Docker REST API, allowing users to perform all container management tasks efficiently.

The CLI is available on Windows, Linux, and macOS and is widely used in automation, CI/CD pipelines, cloud deployments, and Kubernetes workflows.

---

# Learning Objectives

After completing this guide, you will be able to:

* Understand the Docker CLI architecture
* Execute common Docker commands
* Manage images and containers
* Work with Docker networks and volumes
* Troubleshoot Docker CLI issues
* Apply Docker CLI in DevOps workflows

---

# Docker CLI Architecture

```text
                    Docker CLI

          +----------------------+
          |     User Terminal    |
          +----------+-----------+
                     |
             Docker Commands
                     |
                     ▼
          +----------------------+
          |     Docker CLI       |
          +----------+-----------+
                     |
              Docker REST API
                     |
                     ▼
          +----------------------+
          |    Docker Daemon     |
          |      (dockerd)       |
          +----------+-----------+
                     |
      +--------------+----------------+
      |              |                |
      ▼              ▼                ▼
 Docker Images  Docker Containers  Docker Networks
```

---

# Docker CLI Syntax

General syntax:

```bash
docker <command> <options>
```

Example:

```bash
docker ps
```

---

# Docker CLI Command Categories

The Docker CLI provides commands for:

* Image Management
* Container Management
* Network Management
* Volume Management
* Registry Operations
* System Information
* Resource Cleanup

---

# Common Docker CLI Commands

## Display Docker Version

```bash
docker --version
```

Example output:

```text
Docker version 28.x.x
```

---

## Display Detailed Information

```bash
docker info
```

Displays:

* Docker Engine version
* Number of containers
* Number of images
* Storage driver
* CPU
* Memory
* Docker Root Directory

---

## View Help

```bash
docker --help
```

Help for a specific command:

```bash
docker run --help
```

---

# Image Commands

List images:

```bash
docker images
```

Pull an image:

```bash
docker pull nginx
```

Remove an image:

```bash
docker rmi nginx
```

Build an image:

```bash
docker build -t myapp .
```

---

# Container Commands

Run a container:

```bash
docker run nginx
```

Run in detached mode:

```bash
docker run -d nginx
```

List running containers:

```bash
docker ps
```

List all containers:

```bash
docker ps -a
```

Stop a container:

```bash
docker stop <container-id>
```

Start a container:

```bash
docker start <container-id>
```

Restart a container:

```bash
docker restart <container-id>
```

Remove a container:

```bash
docker rm <container-id>
```

Execute commands inside a running container:

```bash
docker exec -it <container-id> bash
```

---

# Network Commands

List networks:

```bash
docker network ls
```

Create a network:

```bash
docker network create app-network
```

Inspect a network:

```bash
docker network inspect app-network
```

Remove a network:

```bash
docker network rm app-network
```

---

# Volume Commands

List volumes:

```bash
docker volume ls
```

Create a volume:

```bash
docker volume create app-data
```

Inspect a volume:

```bash
docker volume inspect app-data
```

Remove a volume:

```bash
docker volume rm app-data
```

---

# System Commands

Display Docker disk usage:

```bash
docker system df
```

Remove unused resources:

```bash
docker system prune
```

Remove everything unused:

```bash
docker system prune -a
```

---

# Docker CLI Workflow

```text
Developer

     │

Docker CLI

     │

Docker Daemon

     │

Create Image

     │

Create Container

     │

Run Application
```

---

# Real-World DevOps Example

A developer builds a web application.

Build the image:

```bash
docker build -t company/web:v1 .
```

Run the application:

```bash
docker run -d -p 8080:80 company/web:v1
```

View running containers:

```bash
docker ps
```

Check logs:

```bash
docker logs <container-id>
```

Push the image:

```bash
docker push company/web:v1
```

The image can now be deployed through a CI/CD pipeline to Kubernetes or another container platform.

---

# Best Practices

* Use descriptive image tags.
* Remove unused images regularly.
* Keep Docker updated.
* Verify commands before running them in production.
* Prefer official images from trusted registries.
* Use detached mode (`-d`) for long-running services.
* Use `docker exec` instead of modifying containers directly.

---

# Common Mistakes

❌ Running containers without naming them.

❌ Forgetting to stop unused containers.

❌ Using `latest` for production deployments.

❌ Removing images still in use.

❌ Ignoring Docker logs when troubleshooting.

---

# Troubleshooting

## Docker command not found

Verify Docker is installed:

```bash
docker --version
```

---

## Cannot connect to Docker daemon

Check Docker status:

```bash
docker info
```

Start Docker Desktop or restart the Docker service.

---

## View Container Logs

```bash
docker logs <container-id>
```

---

## Check Running Containers

```bash
docker ps
```

---

## Inspect Container

```bash
docker inspect <container-id>
```

---

# Interview Questions

### What is Docker CLI?

Docker CLI is the command-line interface used to communicate with Docker Engine for building images, managing containers, networks, and volumes.

---

### How does Docker CLI communicate with Docker Engine?

Through the Docker REST API.

---

### What command lists running containers?

```bash
docker ps
```

---

### What command builds a Docker image?

```bash
docker build -t image-name .
```

---

### What command executes a shell inside a running container?

```bash
docker exec -it <container-id> bash
```

---

# Summary

The Docker CLI is the primary interface for managing Docker environments. It provides a comprehensive set of commands for working with images, containers, networks, volumes, and registries. Mastering the Docker CLI is essential for DevOps engineers because it forms the foundation of container management, automation, CI/CD pipelines, and cloud-native application deployment.

---

# References

Official Docker CLI Documentation

https://docs.docker.com/reference/cli/

Docker Command Reference

https://docs.docker.com/engine/reference/commandline/

Docker Documentation

https://docs.docker.com/
