# Docker Images

## Overview

A Docker image is a lightweight, read-only template that contains everything required to run an application. It includes the application code, runtime, system libraries, dependencies, configuration files, and necessary tools.

Docker images are the foundation of containerization. Every Docker container is created from an image.

Think of a Docker image as a blueprint or template, while a Docker container is the running instance of that blueprint.

---

# What is a Docker Image?

A Docker image is an immutable package that contains:

* Application source code
* Runtime environment
* System libraries
* Dependencies
* Configuration files
* Startup commands

Since images are read-only, they cannot be modified after they are built. Any changes require creating a new image.

---

# Image vs Container

| Docker Image                | Docker Container                     |
| --------------------------- | ------------------------------------ |
| Read-only template          | Running instance of an image         |
| Immutable                   | Can change while running             |
| Used to create containers   | Executes the application             |
| Can be stored in a registry | Exists only after being created      |
| Built once                  | Can be started, stopped, and removed |

Example:

```text
Docker Image
      │
      ▼
docker run
      │
      ▼
Docker Container
```

One Docker image can create multiple containers.

---

# How Docker Images Work

Docker images are stored locally on your machine or in a remote registry such as Docker Hub.

When you run:

```bash
docker run nginx
```

Docker performs the following steps:

1. Checks if the `nginx` image exists locally.
2. Downloads the image from Docker Hub if it is missing.
3. Creates a new container.
4. Starts the container.

This process happens automatically.

---

# Docker Image Architecture

Docker images are built using **layers**.

Each instruction in a Dockerfile creates a new read-only layer.

Example:

```text
Application Files
        │
Configuration
        │
Dependencies
        │
Ubuntu Base Image
        │
Linux Kernel (Host OS)
```

Layered architecture provides several advantages:

* Faster builds
* Reduced storage usage
* Efficient image sharing
* Better caching
* Faster downloads

---

# Docker Image Lifecycle

```text
Dockerfile
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
Docker Container
     │
     ▼
docker stop
     │
     ▼
docker rm
```

---

# Pull an Image

Download an image from Docker Hub.

```bash
docker pull nginx
```

Example output:

```text
Using default tag: latest
latest: Pulling from library/nginx
Digest: sha256:...
Status: Downloaded newer image for nginx:latest
```

---

# List Images

Display all images stored locally.

```bash
docker images
```

or

```bash
docker image ls
```

Example output:

```text
REPOSITORY   TAG       IMAGE ID       CREATED      SIZE
nginx        latest    2f1cd90e      3 weeks ago  192MB
ubuntu       latest    a04dc485      2 weeks ago   78MB
```

### Output Explanation

| Column     | Description             |
| ---------- | ----------------------- |
| Repository | Image name              |
| Tag        | Image version           |
| Image ID   | Unique image identifier |
| Created    | Image creation time     |
| Size       | Total image size        |

---

# Search for Images

Search Docker Hub for available images.

```bash
docker search nginx
```

Example output:

```text
NAME          DESCRIPTION                  STARS
nginx         Official build of Nginx      21000
nginxinc      NGINX Official Image          500
```

---

# Inspect an Image

Display detailed information.

```bash
docker inspect nginx
```

This command provides:

* Image ID
* Creation date
* Environment variables
* Default command
* Labels
* Architecture
* Operating system

---

# View Image History

Display all layers used to build an image.

```bash
docker history nginx
```

Example:

```text
IMAGE        CREATED      CREATED BY
2f1cd90e     3 weeks ago  CMD ["nginx"]
```

This helps understand how an image was built.

---

# Remove an Image

Delete an image from your local system.

```bash
docker rmi nginx
```

or

```bash
docker image rm nginx
```

If the image is being used by a container, Docker displays an error.

Stop and remove the container before deleting the image.

---

# Remove Unused Images

Delete dangling images.

```bash
docker image prune
```

Remove all unused images.

```bash
docker image prune -a
```

This helps reclaim disk space.

---

# Image Tags

Docker images use tags to represent versions.

Example:

```bash
docker pull nginx:latest
```

or

```bash
docker pull nginx:1.27
```

Using version-specific tags ensures consistent deployments across environments.

---

# Official vs Community Images

Docker Hub contains two primary types of images.

### Official Images

* Maintained by Docker or software vendors
* Well-documented
* Frequently updated
* Recommended for production

Examples:

* nginx
* ubuntu
* redis
* mysql
* postgres

### Community Images

* Published by individuals or organizations
* Quality varies
* May contain custom configurations
* Verify the publisher before use

---

# Practical Example

Download the Nginx image.

```bash
docker pull nginx
```

Verify the image.

```bash
docker images
```

Run a container.

```bash
docker run -d --name my-nginx -p 8080:80 nginx
```

Verify that the container is running.

```bash
docker ps
```

Remove the container.

```bash
docker stop my-nginx
docker rm my-nginx
```

Remove the image.

```bash
docker rmi nginx
```

---

# Common Docker Image Commands

| Command                 | Description              |
| ----------------------- | ------------------------ |
| `docker pull nginx`     | Download an image        |
| `docker images`         | List local images        |
| `docker image ls`       | List local images        |
| `docker search nginx`   | Search Docker Hub        |
| `docker inspect nginx`  | Display image details    |
| `docker history nginx`  | Display image layers     |
| `docker rmi nginx`      | Remove an image          |
| `docker image prune`    | Remove dangling images   |
| `docker image prune -a` | Remove all unused images |

---

# Best Practices

* Use official images whenever possible.
* Avoid using the `latest` tag in production.
* Remove unused images regularly.
* Keep images as small as possible.
* Pull updated images to receive security patches.
* Use version-specific tags for predictable deployments.

---

# Common Mistakes

## Confusing Images with Containers

Remember:

* **Image = Blueprint**
* **Container = Running Instance**

Removing a container does not remove the image.

---

## Using the Latest Tag in Production

Avoid:

```bash
docker pull nginx:latest
```

Instead, use:

```bash
docker pull nginx:1.27
```

Versioned images provide consistent deployments.

---

## Removing an Image Used by a Container

Attempting to remove an image while a container is using it results in an error.

Remove the container first.

```bash
docker stop my-nginx
docker rm my-nginx
docker rmi nginx
```

---

# Summary

Docker images are the foundation of containerized applications. They are lightweight, immutable templates that package everything needed to run software consistently across different environments. By understanding how images are built, stored, managed, and versioned, you establish the foundation for creating custom images with Dockerfiles, managing registries, and deploying applications in modern DevOps workflows.
