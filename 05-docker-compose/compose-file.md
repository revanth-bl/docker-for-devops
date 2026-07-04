# Docker Compose File (compose.yaml)

## Overview

A **Docker Compose file** (`compose.yaml`) is a YAML configuration file that defines all the services, networks, volumes, environment variables, and other settings required to run a multi-container Docker application.

Instead of configuring containers manually with multiple `docker run` commands, you describe the entire application in a single file, allowing Docker Compose to build and manage everything automatically.

The Compose file serves as the blueprint for your application's infrastructure.

---

# Why Use a Compose File?

Without a Compose file:

- Every container must be started manually.
- Networks must be created manually.
- Volumes must be created manually.
- Environment variables must be specified for each container.
- Managing multiple services becomes difficult.

With a Compose file:

- One file defines the complete application.
- Infrastructure becomes reproducible.
- Easy collaboration between developers.
- Faster deployments.
- Better organization.

---

# File Names

Docker automatically detects either of these filenames:

```text
compose.yaml
```

or

```text
docker-compose.yml
```

The recommended filename is:

```text
compose.yaml
```

---

# YAML Syntax

Docker Compose files use **YAML (YAML Ain't Markup Language)**.

YAML is indentation-sensitive.

Example:

```yaml
services:
  web:
    image: nginx
```

Incorrect indentation:

```yaml
services:
web:
image: nginx
```

Always use spaces instead of tabs.

---

# Basic Structure

A Compose file generally contains four main sections.

```yaml
services:

volumes:

networks:

configs:
```

The **services** section is mandatory.

---

# Services

The **services** section defines every container in the application.

Example:

```yaml
services:

  web:
    image: nginx:1.27

  database:
    image: mysql:8.4
```

Each service becomes its own Docker container.

---

# Image

Use the **image** key to specify which Docker image should be used.

```yaml
services:

  nginx:

    image: nginx:1.27
```

Docker downloads the image if it is not available locally.

---

# Build

Instead of downloading an image, Docker can build one from a Dockerfile.

```yaml
services:

  app:

    build: .
```

Specify another Dockerfile.

```yaml
services:

  app:

    build:
      context: .
      dockerfile: Dockerfile.dev
```

---

# Ports

Publish container ports.

```yaml
services:

  nginx:

    image: nginx

    ports:
      - "8080:80"
```

Meaning:

```text
Host Port 8080
       │
       ▼
Container Port 80
```

---

# Environment Variables

Provide configuration values.

```yaml
services:

  mysql:

    image: mysql:8.4

    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: appdb
```

Applications read these values during startup.

---

# Volumes

Persist application data.

```yaml
services:

  mysql:

    image: mysql:8.4

    volumes:
      - mysql-data:/var/lib/mysql

volumes:

  mysql-data:
```

The database keeps its data even if the container is recreated.

---

# Networks

Connect services together.

```yaml
services:

  web:

    image: nginx

    networks:
      - app-network

  api:

    image: node:20

    networks:
      - app-network

networks:

  app-network:
```

Containers communicate using service names.

---

# Container Name

Assign a custom container name.

```yaml
services:

  nginx:

    image: nginx

    container_name: my-nginx
```

Without this option, Docker automatically generates a name.

---

# Restart Policy

Automatically restart containers.

```yaml
services:

  nginx:

    image: nginx

    restart: unless-stopped
```

Available policies:

```text
no
always
on-failure
unless-stopped
```

---

# Depends On

Specify startup order.

```yaml
services:

  web:

    depends_on:
      - database

  database:

    image: mysql:8.4
```

Docker starts the database before the web service.

---

# Command

Override the default container command.

```yaml
services:

  app:

    image: ubuntu

    command: sleep infinity
```

---

# Working Directory

Specify the container's working directory.

```yaml
services:

  app:

    image: node:20

    working_dir: /app
```

---

# Complete Compose File

```yaml
services:

  web:

    image: nginx:1.27

    container_name: nginx-web

    ports:
      - "8080:80"

    restart: unless-stopped

    networks:
      - app-network

  database:

    image: mysql:8.4

    container_name: mysql-db

    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: appdb

    volumes:
      - mysql-data:/var/lib/mysql

    restart: unless-stopped

    networks:
      - app-network

volumes:

  mysql-data:

networks:

  app-network:
```

---

# Compose File Hierarchy

```text
compose.yaml
│
├── services
│      ├── web
│      ├── database
│      └── redis
│
├── volumes
│      └── mysql-data
│
└── networks
       └── app-network
```

---

# Common Compose Keys

| Key | Description |
|------|-------------|
| `services` | Defines application containers |
| `image` | Docker image |
| `build` | Build from Dockerfile |
| `ports` | Publish ports |
| `volumes` | Persistent storage |
| `networks` | Connect services |
| `environment` | Environment variables |
| `container_name` | Custom container name |
| `depends_on` | Service startup order |
| `restart` | Restart policy |
| `command` | Override startup command |
| `working_dir` | Working directory inside the container |

---

# Best Practices

- Use `compose.yaml` as the filename.
- Organize services logically.
- Use version-specific image tags.
- Store secrets in `.env` files instead of hardcoding them.
- Use named volumes for persistent data.
- Create custom networks for service communication.
- Keep the file clean and properly indented.
- Use meaningful service names.

---

# Common Mistakes

## Incorrect Indentation

YAML depends on proper indentation.

Incorrect:

```yaml
services:
web:
image: nginx
```

Correct:

```yaml
services:
  web:
    image: nginx
```

---

## Using Latest Images

Avoid:

```yaml
image: nginx:latest
```

Prefer:

```yaml
image: nginx:1.27
```

---

## Forgetting Volumes

Without volumes, application data is lost when containers are removed.

---

## Hardcoding Secrets

Avoid storing passwords directly inside the Compose file.

Instead, use:

```text
.env
```

---

## Not Creating Networks

Use custom networks for multi-container applications instead of relying on the default network.

---

# Summary

A Docker Compose file (`compose.yaml`) is the blueprint for a Docker application. It defines containers, images, networks, volumes, environment variables, and other configuration settings in a single YAML file. By centralizing infrastructure configuration, Compose files make applications easier to deploy, maintain, and share, making them an essential component of modern DevOps workflows.