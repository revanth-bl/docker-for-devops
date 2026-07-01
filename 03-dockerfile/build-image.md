# Build Docker Image

## Overview

The `docker build` command is used to create a Docker image from a Dockerfile. During the build process, Docker reads the instructions in the Dockerfile one by one and creates a new image layer for each instruction.

Building images allows developers and DevOps engineers to package applications, dependencies, and configurations into a portable and reproducible format that can be deployed consistently across different environments.

---

# Why Build Docker Images?

Instead of manually installing software every time you deploy an application, you can package everything into a Docker image.

Benefits include:

* Consistent deployments
* Faster application setup
* Reproducible environments
* Easy version control
* Simplified CI/CD integration
* Portability across systems

---

# Docker Build Syntax

```bash
docker build [OPTIONS] PATH
```

Example:

```bash
docker build -t my-app .
```

Explanation:

* `docker build` → Builds a Docker image.
* `-t` → Assigns a name (tag) to the image.
* `my-app` → Image name.
* `.` → Current directory containing the Dockerfile.

---

# Build Process

```text
Application Files
        │
        ▼
   Dockerfile
        │
docker build
        │
        ▼
 Docker Image
        │
docker run
        │
        ▼
Docker Container
```

---

# Project Structure

Example project:

```text
my-app/
│
├── Dockerfile
├── app.py
├── requirements.txt
└── templates/
```

Docker automatically looks for a file named **Dockerfile** in the current directory.

---

# Example Dockerfile

```dockerfile
FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["python", "app.py"]
```

This Dockerfile:

* Uses Python 3.12 Slim as the base image.
* Sets `/app` as the working directory.
* Installs project dependencies.
* Copies application files.
* Exposes port 5000.
* Starts the application.

---

# Build an Image

Navigate to the project directory.

```bash
cd my-app
```

Build the image.

```bash
docker build -t my-app .
```

Example output:

```text
[+] Building 12.4s
 => FROM python:3.12-slim
 => WORKDIR /app
 => COPY requirements.txt
 => RUN pip install
 => COPY .
 => exporting layers
 => naming to docker.io/library/my-app
```

Docker builds the image layer by layer.

---

# Verify the Image

List available images.

```bash
docker images
```

Example output:

```text
REPOSITORY   TAG      IMAGE ID       CREATED          SIZE
my-app       latest   9f8b12cd34ab   10 seconds ago   180MB
```

---

# Run the Built Image

Start a container using the image.

```bash
docker run -d -p 5000:5000 --name my-container my-app
```

Explanation:

* `-d` → Run in detached mode.
* `-p` → Map host port to container port.
* `--name` → Assign a container name.
* `my-app` → Image name.

---

# Tagging Images

Docker images can have version tags.

```bash
docker build -t my-app:v1 .
```

Create another version.

```bash
docker build -t my-app:v2 .
```

View available versions.

```bash
docker images
```

Example:

```text
REPOSITORY   TAG
my-app       v1
my-app       v2
```

Using tags makes version management easier.

---

# Build Without Cache

Docker caches image layers to speed up builds.

To ignore the cache:

```bash
docker build --no-cache -t my-app .
```

This forces Docker to rebuild every layer from scratch.

---

# Specify a Dockerfile

If your Dockerfile has a different name:

```bash
docker build -f Dockerfile.dev -t my-app .
```

Docker uses the specified file instead of the default `Dockerfile`.

---

# Build from Another Directory

You can specify a different build context.

```bash
docker build -t my-app /home/user/project
```

Docker uses the provided directory as the build context.

---

# Build with Build Arguments

Pass variables during the build process.

Dockerfile:

```dockerfile
ARG APP_VERSION

ENV VERSION=$APP_VERSION
```

Build command:

```bash
docker build --build-arg APP_VERSION=1.0 -t my-app .
```

Build arguments are useful for customizing images during the build process.

---

# Practical Example

Create a Dockerfile.

```dockerfile
FROM nginx:1.27

COPY index.html /usr/share/nginx/html/

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

Build the image.

```bash
docker build -t my-nginx .
```

Verify.

```bash
docker images
```

Run the container.

```bash
docker run -d -p 8080:80 --name my-nginx my-nginx
```

Open your browser.

```text
http://localhost:8080
```

Your custom web page should now be displayed.

---

# Common Docker Build Commands

| Command                                      | Description                       |
| -------------------------------------------- | --------------------------------- |
| `docker build -t my-app .`                   | Build an image                    |
| `docker build --no-cache -t my-app .`        | Build without using cache         |
| `docker build -f Dockerfile.dev -t my-app .` | Use a custom Dockerfile           |
| `docker build -t my-app:v1 .`                | Build an image with a version tag |
| `docker images`                              | List local images                 |
| `docker history my-app`                      | Display image layers              |
| `docker inspect my-app`                      | Show image details                |

---

# Best Practices

* Use meaningful image names.
* Tag images with version numbers instead of relying on `latest`.
* Keep the build context small by using a `.dockerignore` file.
* Use lightweight base images such as Alpine or Slim variants.
* Combine related commands to reduce image layers.
* Clean package caches after installing dependencies.
* Regularly rebuild images to receive security updates.

---

# Common Mistakes

## Forgetting the Build Context

Incorrect:

```bash
docker build -t my-app
```

Correct:

```bash
docker build -t my-app .
```

The `.` specifies the current directory as the build context.

---

## Missing Dockerfile

If Docker cannot find a Dockerfile, it returns an error similar to:

```text
failed to read dockerfile: no such file or directory
```

Ensure the Dockerfile exists in the specified directory.

---

## Using the Latest Tag

Avoid:

```bash
docker build -t my-app:latest .
```

Prefer:

```bash
docker build -t my-app:v1.0 .
```

Versioned tags make deployments more predictable.

---

## Large Build Context

Copying unnecessary files increases build time and image size.

Use a `.dockerignore` file to exclude:

* `.git`
* `node_modules`
* `*.log`
* `.env`
* Temporary files

---

# Summary

The `docker build` command is used to create Docker images from a Dockerfile. It automates application packaging by combining source code, dependencies, and configuration into a portable image. Understanding how to build, tag, and optimize Docker images is a fundamental DevOps skill and the foundation for deploying applications in containers, CI/CD pipelines, and cloud environments.
