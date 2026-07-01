# Dockerfile

## Overview

A **Dockerfile** is a text file that contains a set of instructions used by Docker to automatically build a Docker image. Instead of manually installing software and configuring environments every time, a Dockerfile allows you to define the entire setup as code.

Docker reads each instruction in the Dockerfile from top to bottom and creates a new image layer for every instruction. This makes image creation reproducible, consistent, and easy to automate.

Dockerfiles are an essential part of modern DevOps workflows because they enable Infrastructure as Code (IaC) principles for application environments.

---

# Why Use a Dockerfile?

Without a Dockerfile, you would need to:

* Install dependencies manually.
* Configure the operating system.
* Copy application files.
* Start the application manually.

This process is time-consuming and error-prone.

A Dockerfile automates all these tasks, ensuring that every image is built the same way every time.

Benefits include:

* Consistent environments
* Faster deployments
* Version-controlled infrastructure
* Easy collaboration
* Automated CI/CD pipelines

---

# Dockerfile Workflow

```text
Application Source Code
          │
          ▼
      Dockerfile
          │
docker build -t my-app .
          │
          ▼
     Docker Image
          │
docker run my-app
          │
          ▼
    Docker Container
```

---

# Basic Dockerfile Structure

Example:

```dockerfile
FROM ubuntu:22.04

RUN apt-get update

RUN apt-get install -y nginx

COPY . /app

WORKDIR /app

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

Each instruction performs a specific task while building the image.

---

# Common Dockerfile Instructions

| Instruction  | Description                                        |
| ------------ | -------------------------------------------------- |
| `FROM`       | Specifies the base image                           |
| `RUN`        | Executes commands during image build               |
| `COPY`       | Copies files from the host into the image          |
| `ADD`        | Copies files and supports remote URLs and archives |
| `WORKDIR`    | Sets the working directory                         |
| `EXPOSE`     | Documents the container's listening port           |
| `ENV`        | Defines environment variables                      |
| `CMD`        | Specifies the default command                      |
| `ENTRYPOINT` | Defines the main executable                        |
| `USER`       | Specifies the user for running the container       |
| `LABEL`      | Adds image metadata                                |

---

# Docker Image Layers

Each Dockerfile instruction creates a new read-only layer.

Example:

```text
Application Files
──────────────────
COPY
──────────────────
RUN apt-get install
──────────────────
FROM ubuntu
```

Benefits of image layers:

* Faster image builds
* Efficient caching
* Reduced storage usage
* Faster downloads
* Layer reuse across images

---

# Creating Your First Dockerfile

Create a file named:

```text
Dockerfile
```

Example:

```dockerfile
FROM nginx:latest

COPY index.html /usr/share/nginx/html/

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

---

# Build the Docker Image

```bash
docker build -t my-nginx .
```

Explanation:

* `docker build` → Build an image.
* `-t` → Assign a name (tag) to the image.
* `.` → Current directory containing the Dockerfile.

---

# Verify the Image

```bash
docker images
```

Example output:

```text
REPOSITORY    TAG       IMAGE ID       SIZE
my-nginx      latest    ab12cd34ef56   195MB
```

---

# Run the Image

```bash
docker run -d -p 8080:80 --name my-nginx-container my-nginx
```

Open your browser:

```text
http://localhost:8080
```

Your application should now be running inside a Docker container.

---

# Dockerfile vs Docker Image

| Dockerfile                  | Docker Image                   |
| --------------------------- | ------------------------------ |
| Text file                   | Binary package                 |
| Contains build instructions | Contains the built application |
| Human-readable              | Executable by Docker           |
| Version controlled          | Used to create containers      |

---

# Best Practices

* Keep Dockerfiles simple and readable.
* Use official base images whenever possible.
* Pin image versions instead of using `latest`.
* Minimize the number of image layers.
* Use `.dockerignore` to exclude unnecessary files.
* Keep one application per container.
* Store configuration in environment variables.

---

# Common Mistakes

### Using the Latest Tag

Avoid:

```dockerfile
FROM ubuntu:latest
```

Instead:

```dockerfile
FROM ubuntu:22.04
```

---

### Copying Everything

Avoid:

```dockerfile
COPY . .
```

without a `.dockerignore` file, as it may include unnecessary files such as Git metadata, logs, or secrets.

---

### Multiple Applications in One Container

A Docker container should ideally run a single primary process.

---

# Summary

A Dockerfile is the blueprint used to build Docker images. It automates application packaging by defining every required step—from selecting a base image to copying files and specifying the startup command. Mastering Dockerfiles is fundamental for creating reproducible, portable, and production-ready containerized applications, making it one of the most important skills for any DevOps engineer.
