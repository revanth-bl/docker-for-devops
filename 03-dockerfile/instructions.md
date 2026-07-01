# Dockerfile Instructions

## Overview

A Dockerfile consists of a series of instructions that Docker executes sequentially to build a Docker image. Each instruction performs a specific task, such as selecting a base image, copying files, installing dependencies, or defining how the container should start.

Most Dockerfiles use a combination of these instructions to create lightweight, reproducible, and production-ready Docker images.

---

# Dockerfile Execution Flow

Docker reads a Dockerfile from top to bottom.

```text id="ml6ll8"
FROM
   │
RUN
   │
COPY
   │
WORKDIR
   │
ENV
   │
EXPOSE
   │
CMD
   │
Docker Image
```

Each instruction creates a new image layer, except for metadata instructions such as `LABEL`.

---

# FROM

The `FROM` instruction specifies the base image from which the new image will be built.

Every Dockerfile must begin with a `FROM` instruction.

Syntax:

```dockerfile id="2x7vph"
FROM ubuntu:22.04
```

Example:

```dockerfile id="9m7ovm"
FROM python:3.12-slim
```

This tells Docker to use the official Python 3.12 Slim image as the base.

---

# RUN

The `RUN` instruction executes commands while building the image.

It is commonly used to:

* Install software
* Update packages
* Create directories
* Configure the system

Example:

```dockerfile id="t76qfm"
RUN apt-get update
```

Install multiple packages:

```dockerfile id="xohzfm"
RUN apt-get update && \
    apt-get install -y nginx curl git
```

Using a single `RUN` instruction reduces the number of image layers.

---

# COPY

The `COPY` instruction copies files from the local machine into the Docker image.

Syntax:

```dockerfile id="vjlwm6"
COPY source destination
```

Example:

```dockerfile id="jlwmij"
COPY . /app
```

Copy a single file:

```dockerfile id="cqilzg"
COPY requirements.txt .
```

COPY is the preferred method for adding project files to an image.

---

# ADD

The `ADD` instruction is similar to `COPY` but provides additional functionality.

It can:

* Extract compressed archives
* Download files from URLs

Example:

```dockerfile id="rr9n0n"
ADD app.tar.gz /app
```

Although powerful, Docker recommends using `COPY` unless the extra features of `ADD` are required.

---

# WORKDIR

The `WORKDIR` instruction sets the default working directory.

Example:

```dockerfile id="i5mznz"
WORKDIR /app
```

All subsequent commands execute from this directory.

Instead of repeatedly writing:

```dockerfile id="wqocds"
RUN cd /app
```

use:

```dockerfile id="rq0qor"
WORKDIR /app
```

---

# ENV

The `ENV` instruction defines environment variables.

Example:

```dockerfile id="6rjlwm"
ENV PORT=5000
```

Multiple variables:

```dockerfile id="vtfz3t"
ENV APP_ENV=production

ENV DEBUG=False
```

Applications can read these variables during runtime.

---

# EXPOSE

The `EXPOSE` instruction documents which port the application listens on.

Example:

```dockerfile id="j8d10q"
EXPOSE 80
```

or

```dockerfile id="yn3mne"
EXPOSE 5000
```

> **Note:** `EXPOSE` does not publish ports. Port mapping is performed using the `-p` option with `docker run`.

---

# CMD

The `CMD` instruction specifies the default command that runs when a container starts.

Example:

```dockerfile id="kkm8ow"
CMD ["python", "app.py"]
```

For Nginx:

```dockerfile id="h6qljo"
CMD ["nginx", "-g", "daemon off;"]
```

A Dockerfile should contain only one `CMD` instruction. If multiple `CMD` instructions are present, only the last one is used.

---

# ENTRYPOINT

`ENTRYPOINT` defines the main executable for the container.

Example:

```dockerfile id="qmjlwm"
ENTRYPOINT ["python"]
```

Combined with CMD:

```dockerfile id="y86lra"
ENTRYPOINT ["python"]

CMD ["app.py"]
```

This allows additional arguments to be passed when starting the container.

---

# LABEL

The `LABEL` instruction adds metadata to an image.

Example:

```dockerfile id="jlwmzn"
LABEL maintainer="John Doe"

LABEL version="1.0"

LABEL description="Docker Portfolio Project"
```

Labels make images easier to identify and manage.

---

# USER

The `USER` instruction specifies which user runs the application.

Example:

```dockerfile id="xjlwmr"
RUN useradd appuser

USER appuser
```

Running containers as a non-root user improves security.

---

# ARG

The `ARG` instruction defines build-time variables.

Example:

```dockerfile id="jlwm5h"
ARG VERSION=1.0
```

Build command:

```bash id="jlwm3d"
docker build --build-arg VERSION=2.0 -t my-app .
```

Unlike `ENV`, build arguments are available only during image creation.

---

# VOLUME

The `VOLUME` instruction creates a mount point for persistent data.

Example:

```dockerfile id="jlwm6r"
VOLUME ["/data"]
```

Volumes preserve data even if the container is removed.

---

# Complete Example

```dockerfile id="jlwm8v"
FROM python:3.12-slim

LABEL maintainer="Your Name"

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

ENV PORT=5000

EXPOSE 5000

USER nobody

CMD ["python", "app.py"]
```

This Dockerfile:

* Uses an official Python image
* Adds metadata
* Sets a working directory
* Installs dependencies
* Copies application files
* Defines an environment variable
* Documents the application port
* Runs as a non-root user
* Starts the application

---

# Most Common Dockerfile Instructions

| Instruction  | Purpose                         |
| ------------ | ------------------------------- |
| `FROM`       | Base image                      |
| `RUN`        | Execute commands during build   |
| `COPY`       | Copy files into the image       |
| `ADD`        | Copy files and extract archives |
| `WORKDIR`    | Set the working directory       |
| `ENV`        | Define environment variables    |
| `ARG`        | Define build-time variables     |
| `EXPOSE`     | Document application ports      |
| `CMD`        | Default startup command         |
| `ENTRYPOINT` | Main executable                 |
| `LABEL`      | Add metadata                    |
| `USER`       | Specify the execution user      |
| `VOLUME`     | Create persistent storage       |

---

# Best Practices

* Start with an official base image.
* Use specific image versions instead of `latest`.
* Combine `RUN` commands to reduce image layers.
* Prefer `COPY` over `ADD` unless archive extraction or remote URLs are needed.
* Use `WORKDIR` instead of repeatedly changing directories.
* Store configuration in environment variables.
* Run containers as a non-root user.
* Keep Dockerfiles simple, readable, and maintainable.

---

# Common Mistakes

### Multiple CMD Instructions

Incorrect:

```dockerfile id="jlwmii"
CMD ["python", "app.py"]

CMD ["echo", "Hello"]
```

Only the second `CMD` is used.

---

### Forgetting WORKDIR

Without a working directory, files may be copied to unexpected locations.

Always define:

```dockerfile id="jlwmpp"
WORKDIR /app
```

---

### Running as Root

Avoid running production containers as the root user.

Create a dedicated application user instead.

---

# Summary

Dockerfile instructions are the building blocks used to create Docker images. Each instruction performs a specific task, such as selecting a base image, installing dependencies, copying files, setting environment variables, or defining how a container starts. Understanding these instructions is essential for writing efficient, secure, and production-ready Dockerfiles in modern DevOps workflows.
