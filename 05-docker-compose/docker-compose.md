# Docker Compose File

## Overview

A **Docker Compose file** is a YAML configuration file (`compose.yaml` or `docker-compose.yml`) that defines how multiple Docker containers should be built, configured, networked, and started together.

Instead of running several long `docker run` commands manually, Docker Compose allows you to describe the entire application in a single file and start it with one command.

Docker Compose is widely used for:

- Multi-container applications
- Development environments
- Testing environments
- CI/CD pipelines
- Microservices

---

# Why Use a Compose File?

Without Docker Compose:

```bash
docker network create app-network

docker volume create mysql-data

docker run -d \
--name mysql \
--network app-network \
-v mysql-data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=password \
mysql:8.4

docker run -d \
--name web \
--network app-network \
-p 8080:80 \
nginx
```

Managing multiple commands becomes difficult.

With Docker Compose:

```bash
docker compose up -d
```

Everything is created automatically.

---

# Docker Compose File Structure

A typical Compose file contains:

- Version (optional in newer Compose versions)
- Services
- Networks
- Volumes

Example structure:

```yaml
services:
  service-name:
    image:
    ports:
    volumes:
    environment:

volumes:

networks:
```

---

# File Name

Docker automatically detects:

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

# Simple Compose File

```yaml
services:
  nginx:
    image: nginx:1.27
    ports:
      - "8080:80"
```

Start it.

```bash
docker compose up -d
```

Visit:

```text
http://localhost:8080
```

---

# Compose File with Multiple Services

```yaml
services:

  web:
    image: nginx:1.27
    ports:
      - "8080:80"

  database:
    image: mysql:8.4
    environment:
      MYSQL_ROOT_PASSWORD: password
```

Docker starts both containers together.

---

# Build Instead of Image

Instead of downloading an image, Docker can build one.

```yaml
services:

  app:
    build: .
```

Docker reads the Dockerfile in the current directory.

Specify another Dockerfile.

```yaml
services:

  app:
    build:
      context: .
      dockerfile: Dockerfile.dev
```

---

# Port Mapping

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

```yaml
services:

  mysql:
    image: mysql:8.4

    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: appdb
```

Docker automatically passes these variables into the container.

---

# Volumes

```yaml
services:

  mysql:
    image: mysql:8.4

    volumes:
      - mysql-data:/var/lib/mysql

volumes:
  mysql-data:
```

Data remains even if the container is removed.

---

# Networks

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

Both containers can communicate using service names.

---

# Restart Policy

Automatically restart containers.

```yaml
services:

  nginx:
    image: nginx

    restart: always
```

Other options:

```text
no
always
on-failure
unless-stopped
```

---

# Container Name

Assign a custom container name.

```yaml
services:

  nginx:
    image: nginx

    container_name: my-nginx
```

---

# Complete Example

```yaml
services:

  web:
    image: nginx:1.27

    container_name: nginx-web

    ports:
      - "8080:80"

    restart: unless-stopped

  database:
    image: mysql:8.4

    container_name: mysql-db

    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: appdb

    volumes:
      - mysql-data:/var/lib/mysql

    restart: unless-stopped

volumes:
  mysql-data:
```

Run the application.

```bash
docker compose up -d
```

View containers.

```bash
docker compose ps
```

Stop everything.

```bash
docker compose down
```

---

# Common Compose File Keys

| Key | Purpose |
|------|---------|
| `services` | Define containers |
| `image` | Docker image |
| `build` | Build from Dockerfile |
| `ports` | Publish ports |
| `volumes` | Mount persistent storage |
| `networks` | Connect services |
| `environment` | Environment variables |
| `container_name` | Custom container name |
| `restart` | Restart policy |
| `depends_on` | Start services in dependency order |

---

# Best Practices

- Use `compose.yaml` as the filename.
- Use meaningful service names.
- Store secrets in `.env` files instead of hardcoding them.
- Use named volumes for persistent data.
- Create custom networks for multi-container applications.
- Pin image versions instead of using `latest`.
- Keep the Compose file simple and well organized.

---

# Common Mistakes

## Using Latest Images

Avoid:

```yaml
image: nginx:latest
```

Use:

```yaml
image: nginx:1.27
```

---

## Hardcoding Passwords

Avoid:

```yaml
environment:
  MYSQL_ROOT_PASSWORD: admin123
```

Use a `.env` file instead.

---

## Forgetting Volumes

Without a volume:

```yaml
services:
  mysql:
    image: mysql
```

Database data is lost when the container is removed.

---

## Forgetting Port Mapping

Without:

```yaml
ports:
  - "8080:80"
```

The application cannot be accessed from the host.

---

# Summary

A Docker Compose file is a YAML configuration that defines multi-container Docker applications. It allows developers to describe services, networks, volumes, environment variables, and other settings in a single file, making deployments simpler, repeatable, and easier to manage. Docker Compose is an essential tool for DevOps engineers, enabling consistent local development, testing, and production deployments.