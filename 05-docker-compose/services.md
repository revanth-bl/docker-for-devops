# Services in Docker Compose

## Overview

A **service** in Docker Compose represents a single container or a group of identical containers that perform a specific function within an application.

Each service is defined inside the `compose.yaml` file and includes information such as:

- Docker image
- Build context
- Ports
- Environment variables
- Volumes
- Networks
- Restart policy

When Docker Compose starts an application, it creates one container for each service.

---

# Why Use Services?

Modern applications consist of multiple components.

For example:

- Web Server
- Backend API
- Database
- Cache
- Reverse Proxy

Each component should run independently as its own service.

Benefits include:

- Better organization
- Easier maintenance
- Independent scaling
- Improved fault isolation
- Simplified deployment

---

# Service Architecture

```text
                 Docker Compose
                        │
        ┌───────────────┼───────────────┐
        ▼               ▼               ▼
     Web Service    API Service   Database Service
         │               │               │
     Nginx          Node.js         MySQL
```

Each service becomes its own Docker container.

---

# Defining a Service

Every service is declared under the `services` section.

```yaml
services:

  web:
    image: nginx:1.27
```

Here:

- `web` is the service name.
- `image` specifies the Docker image.

---

# Multiple Services

A Compose file can define multiple services.

```yaml
services:

  web:
    image: nginx:1.27

  api:
    image: node:20

  database:
    image: mysql:8.4
```

Docker Compose starts all services together.

---

# Service Name

Each service must have a unique name.

```yaml
services:

  frontend:
    image: nginx

  backend:
    image: node:20
```

Service names are also used for internal DNS communication.

Example:

```text
backend
```

instead of

```text
172.18.0.5
```

---

# Image

Specify the Docker image.

```yaml
services:

  nginx:

    image: nginx:1.27
```

Docker downloads the image automatically if it is not available locally.

---

# Build

Build an image using a Dockerfile.

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

Expose container ports.

```yaml
services:

  web:

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

Pass configuration into the service.

```yaml
services:

  database:

    image: mysql:8.4

    environment:
      MYSQL_ROOT_PASSWORD: password
```

---

# Volumes

Attach persistent storage.

```yaml
services:

  database:

    image: mysql:8.4

    volumes:
      - mysql-data:/var/lib/mysql

volumes:

  mysql-data:
```

---

# Networks

Connect services.

```yaml
services:

  web:

    image: nginx

    networks:
      - app-network

networks:

  app-network:
```

Containers on the same network communicate using service names.

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

Docker starts the database service before the web service.

---

# Restart Policy

Automatically restart a service.

```yaml
services:

  nginx:

    image: nginx

    restart: unless-stopped
```

Options:

```text
no
always
on-failure
unless-stopped
```

---

# Container Name

Assign a custom name.

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

    depends_on:
      - database

    restart: unless-stopped

    networks:
      - app-network

  database:

    image: mysql:8.4

    container_name: mysql-db

    environment:
      MYSQL_ROOT_PASSWORD: password

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

Start the services.

```bash
docker compose up -d
```

View running services.

```bash
docker compose ps
```

---

# Service Lifecycle

```text
Define Service
       │
       ▼
docker compose up
       │
       ▼
Create Container
       │
       ▼
Running
       │
       ▼
Stop
       │
       ▼
Remove
```

---

# Common Service Commands

| Command | Description |
|----------|-------------|
| `docker compose up -d` | Start all services |
| `docker compose ps` | View running services |
| `docker compose stop` | Stop services |
| `docker compose start` | Start stopped services |
| `docker compose restart` | Restart services |
| `docker compose logs` | View service logs |
| `docker compose exec service-name sh` | Open a shell inside a service |
| `docker compose down` | Remove all services |

---

# Best Practices

- Give services meaningful names.
- Run one primary process per service.
- Use specific image versions instead of `latest`.
- Use named volumes for persistent data.
- Connect related services using custom networks.
- Store configuration in environment variables.
- Use `depends_on` when services rely on one another.
- Keep services small and focused on a single responsibility.

---

# Common Mistakes

## Running Multiple Applications in One Service

Avoid putting Nginx, MySQL, and a backend application into one container.

Create separate services instead.

---

## Using IP Addresses

Avoid:

```text
DB_HOST=172.18.0.5
```

Use:

```text
DB_HOST=database
```

Docker automatically resolves service names.

---

## Forgetting Dependencies

If the API requires a database, define:

```yaml
depends_on:
  - database
```

---

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

# Summary

A service is the fundamental building block of a Docker Compose application. Each service represents a container with its own configuration, including images, ports, networks, volumes, and environment variables. By defining services in a `compose.yaml` file, Docker Compose can deploy, manage, and coordinate multi-container applications consistently and efficiently, making services a core concept in modern DevOps and containerized application development.