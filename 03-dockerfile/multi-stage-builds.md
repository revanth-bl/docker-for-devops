# Multi-Stage Builds

## Overview

A **Multi-Stage Build** is a Dockerfile technique that uses multiple `FROM` instructions to create smaller, cleaner, and more secure Docker images.

Instead of shipping the entire build environment (compilers, package managers, development tools, and source code) into production, Docker copies only the files required to run the application into the final image.

Multi-stage builds are considered a **best practice** and are widely used in production environments.

---

# Why Use Multi-Stage Builds?

When building an application, you usually need:

- Source code
- Build tools
- Package managers
- Compilers
- Dependencies

However, a running application only needs:

- Compiled application
- Runtime libraries
- Configuration files

Multi-stage builds separate these two stages.

Benefits include:

- Smaller Docker images
- Faster deployments
- Reduced attack surface
- Better security
- Lower storage usage
- Faster downloads

---

# Build Process

```text
Source Code
      │
      ▼
Build Stage
(Install Dependencies)
      │
Compile Application
      │
      ▼
Production Stage
(Copy Only Build Output)
      │
      ▼
Small Docker Image
      │
      ▼
Docker Container
```

---

# Basic Syntax

A multi-stage Dockerfile contains multiple `FROM` instructions.

```dockerfile
FROM node:20 AS builder

WORKDIR /app

COPY . .

RUN npm install

RUN npm run build

FROM nginx:1.27

COPY --from=builder /app/dist /usr/share/nginx/html
```

Here:

- First stage builds the application.
- Second stage creates the production image.

---

# Stage Names

Use the `AS` keyword to name stages.

```dockerfile
FROM node:20 AS builder
```

Later:

```dockerfile
COPY --from=builder /app/dist /usr/share/nginx/html
```

Naming stages makes Dockerfiles easier to read and maintain.

---

# Example: Node.js Application

```dockerfile
FROM node:20 AS builder

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build

FROM nginx:1.27

COPY --from=builder /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

### Build Image

```bash
docker build -t react-app .
```

Run the container.

```bash
docker run -d -p 8080:80 react-app
```

Open:

```text
http://localhost:8080
```

---

# Example: Go Application

```dockerfile
FROM golang:1.24 AS builder

WORKDIR /app

COPY . .

RUN go build -o myapp

FROM alpine:3.21

COPY --from=builder /app/myapp .

CMD ["./myapp"]
```

Only the compiled binary is copied into the final image.

---

# Example: Python Application

```dockerfile
FROM python:3.12 AS builder

WORKDIR /app

COPY requirements.txt .

RUN pip install --prefix=/install -r requirements.txt

COPY . .

FROM python:3.12-slim

WORKDIR /app

COPY --from=builder /install /usr/local

COPY . .

CMD ["python", "app.py"]
```

The production image contains only the installed packages and application files.

---

# Single-Stage vs Multi-Stage

## Single-Stage Build

```dockerfile
FROM node:20

WORKDIR /app

COPY . .

RUN npm install

RUN npm run build

CMD ["npm", "start"]
```

Problems:

- Includes Node.js build tools
- Includes source code
- Larger image
- Slower deployment

---

## Multi-Stage Build

```dockerfile
FROM node:20 AS builder

WORKDIR /app

COPY . .

RUN npm install

RUN npm run build

FROM nginx:1.27

COPY --from=builder /app/dist /usr/share/nginx/html
```

Advantages:

- Smaller image
- Faster startup
- Better security
- Cleaner production environment

---

# Docker Build

Build the image.

```bash
docker build -t my-app .
```

Verify:

```bash
docker images
```

Run:

```bash
docker run -d -p 8080:80 my-app
```

---

# Image Size Comparison

| Build Type | Approximate Size |
|------------|-----------------:|
| Single Stage | 900 MB |
| Multi-Stage | 150 MB |

> **Note:** Actual image sizes vary depending on the application and base image.

---

# Common Commands

| Command | Description |
|----------|-------------|
| `docker build -t my-app .` | Build image |
| `docker images` | List images |
| `docker history my-app` | View image layers |
| `docker inspect my-app` | Inspect image details |
| `docker run my-app` | Run container |

---

# Best Practices

- Use multi-stage builds for production applications.
- Keep the final image as small as possible.
- Use lightweight runtime images such as Alpine or Slim variants.
- Copy only the files required to run the application.
- Name build stages using the `AS` keyword.
- Remove unnecessary build tools from the final image.
- Use version-specific base images instead of `latest`.

---

# Common Mistakes

## Copying the Entire Project

Avoid copying unnecessary files into the production image.

Only copy the build output.

---

## Using Build Images in Production

Avoid running production containers from images that include compilers, package managers, or development tools.

---

## Forgetting Stage Names

Instead of:

```dockerfile
COPY --from=0
```

Prefer:

```dockerfile
COPY --from=builder
```

Named stages are easier to understand and maintain.

---

## Using Large Runtime Images

If the application only needs a runtime environment, use lightweight images such as:

```dockerfile
FROM python:3.12-slim
```

or

```dockerfile
FROM alpine:3.21
```

---

# Summary

Multi-stage builds allow Docker images to be built in multiple phases, separating the build environment from the production environment. By copying only the necessary artifacts into the final image, developers can create smaller, faster, and more secure Docker images. Multi-stage builds are considered a production best practice and are widely used in modern DevOps workflows, CI/CD pipelines, and cloud-native applications.