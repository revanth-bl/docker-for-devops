# Multi-Container Application with Docker Compose

## Overview

Modern applications rarely consist of a single container. A typical application includes multiple services working together, such as a web server, backend API, database, cache, and reverse proxy.

Docker Compose simplifies the deployment and management of these services by allowing them to run together as a single application.

A **Multi-Container Application** is a collection of containers that communicate through Docker networks while remaining isolated from one another.

---

# Why Use Multi-Container Applications?

Real-world applications require multiple services.

Example:

- Frontend
- Backend API
- Database
- Cache
- Reverse Proxy

Instead of running each service manually, Docker Compose manages them together.

Benefits include:

- Easier deployment
- Automatic networking
- Centralized configuration
- Simplified scaling
- Better maintainability

---

# Application Architecture

```text
                    User
                      │
                      ▼
                Web Browser
                      │
                      ▼
                 Nginx Web Server
                      │
                      ▼
                 Backend API
                      │
          ┌───────────┴───────────┐
          ▼                       ▼
      MySQL Database         Redis Cache
```

Each service runs inside its own container.

---

# Project Structure

```text
multi-container-app/
│
├── compose.yaml
├── Dockerfile
├── app/
│   ├── app.py
│   └── requirements.txt
│
└── .env
```

---

# Compose File Example

```yaml
services:

  web:

    image: nginx:1.27

    container_name: nginx-web

    ports:
      - "8080:80"

    depends_on:
      - api

    networks:
      - app-network

  api:

    build: .

    container_name: backend-api

    environment:
      DB_HOST: database

    depends_on:
      - database

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

    networks:
      - app-network

volumes:

  mysql-data:

networks:

  app-network:
```

---

# Start the Application

Launch all containers.

```bash
docker compose up -d
```

Docker automatically:

- Creates the network
- Creates the volume
- Builds custom images
- Starts every container

---

# View Running Services

```bash
docker compose ps
```

Example:

```text
NAME           STATUS
nginx-web      Up
backend-api    Up
mysql-db       Up
```

---

# View Logs

Display logs from all services.

```bash
docker compose logs
```

Specific service:

```bash
docker compose logs api
```

Follow logs in real time.

```bash
docker compose logs -f
```

---

# Execute Commands Inside a Container

Open a shell.

```bash
docker compose exec api sh
```

Verify database connectivity or application files from inside the container.

---

# Container Communication

Because every service belongs to the same Docker network, containers communicate using service names.

Example:

```text
API → database
```

Instead of:

```text
API → 172.18.0.2
```

Compose automatically provides DNS resolution.

---

# Scaling Services

Increase the number of application instances.

```bash
docker compose up --scale api=3 -d
```

Docker starts three API containers.

This is useful for testing load balancing and high availability.

---

# Stop the Application

Stop containers.

```bash
docker compose stop
```

Restart.

```bash
docker compose start
```

Stop and remove everything.

```bash
docker compose down
```

Remove everything including volumes.

```bash
docker compose down -v
```

---

# Workflow

```text
Write compose.yaml
        │
        ▼
docker compose up -d
        │
        ▼
Create Network
        │
        ▼
Create Volumes
        │
        ▼
Start Containers
        │
        ▼
Application Running
```

---

# Common Docker Compose Commands

| Command | Description |
|----------|-------------|
| `docker compose up -d` | Start all services |
| `docker compose ps` | View running containers |
| `docker compose logs` | View logs |
| `docker compose exec api sh` | Open a shell inside a container |
| `docker compose stop` | Stop services |
| `docker compose start` | Start stopped services |
| `docker compose restart` | Restart services |
| `docker compose down` | Stop and remove containers |
| `docker compose down -v` | Remove containers and volumes |
| `docker compose up --scale api=3 -d` | Scale a service |

---

# Best Practices

- Keep one service per container.
- Use meaningful service names.
- Store secrets in a `.env` file.
- Use named volumes for persistent data.
- Create custom networks instead of using the default network.
- Pin image versions instead of using `latest`.
- Use `depends_on` to define startup order.
- Regularly check logs for troubleshooting.

---

# Common Mistakes

## Running Everything in One Container

Avoid combining the web server, API, and database into a single container.

Each service should run independently.

---

## Using IP Addresses

Avoid:

```text
DB_HOST=172.18.0.2
```

Use:

```text
DB_HOST=database
```

Docker Compose automatically resolves service names.

---

## Forgetting Persistent Storage

Without volumes:

```yaml
database:
  image: mysql
```

Database data is lost when the container is removed.

---

## Ignoring Logs

Logs help diagnose application issues.

Use:

```bash
docker compose logs -f
```

during development and troubleshooting.

---

# Real-World Examples

Docker Compose is commonly used for:

- Nginx + Node.js + MySQL
- React + Express + MongoDB
- Django + PostgreSQL + Redis
- WordPress + MySQL
- Laravel + MySQL + Redis
- Flask + PostgreSQL + Nginx

---

# Summary

A multi-container application consists of multiple Docker containers working together to provide a complete solution. Docker Compose simplifies the deployment and management of these applications by defining services, networks, volumes, and dependencies in a single `compose.yaml` file. This approach is widely used in development, testing, and production environments, making it an essential skill for DevOps engineers and cloud-native application development.