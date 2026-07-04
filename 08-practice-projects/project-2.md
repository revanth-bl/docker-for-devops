# Project 2 — WordPress and MySQL with Docker Compose

## Objective

Deploy a multi-container WordPress application using Docker Compose.

---

# Technologies

- Docker
- Docker Compose
- WordPress
- MySQL

---

# Architecture

```text
Browser
    │
    ▼
WordPress
    │
    ▼
MySQL
```

---

# compose.yaml

```yaml
services:

  wordpress:

    image: wordpress:latest

    ports:
      - "8080:80"

    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: password
      WORDPRESS_DB_NAME: wordpress

    depends_on:
      - db

  db:

    image: mysql:8.4

    environment:
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: password
      MYSQL_ROOT_PASSWORD: rootpassword

    volumes:
      - db-data:/var/lib/mysql

volumes:

  db-data:
```

---

# Commands

```bash
docker compose up -d

docker compose ps

docker compose logs

docker compose down
```

Visit:

```text
http://localhost:8080
```

Complete the WordPress installation through the browser.

---

# Skills Learned

- Docker Compose
- Multi-container Applications
- Volumes
- Networks
- Environment Variables

---

# Best Practices

- Use named volumes.
- Use `.env` files for credentials.
- Pin image versions instead of using `latest` in production.

---

# Summary

This project demonstrates deploying a real-world multi-container application using Docker Compose, with WordPress connected to a persistent MySQL database.