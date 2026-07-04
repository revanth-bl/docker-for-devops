# Multi-Container Lab

## Overview

This lab demonstrates how multiple Docker containers communicate using Docker Compose.

The application consists of:

- Nginx
- MySQL
- Docker Network
- Docker Volume

---

# Architecture

```text
Browser
    │
    ▼
 Nginx
    │
    ▼
 MySQL
```

---

# Project Structure

```text
multi-container-lab/
│
├── compose.yaml
└── README.md
```

---

# compose.yaml

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

    volumes:
      - mysql-data:/var/lib/mysql

volumes:

  mysql-data:
```

---

# Commands

```bash
docker compose up -d

docker compose ps

docker compose logs

docker compose down
```

---

# Learning Outcomes

- Docker Compose
- Networks
- Volumes
- Multiple Containers

---

# Summary

This lab demonstrates deploying and managing multiple services using Docker Compose.