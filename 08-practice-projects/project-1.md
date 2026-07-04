# Project 1 — Static Website Deployment with Docker

## Objective

Deploy a static HTML website using the official Nginx Docker image.

---

# Technologies

- Docker
- Nginx
- HTML
- CSS

---

# Architecture

```text
Browser
    │
    ▼
Docker Container
    │
    ▼
Nginx
    │
    ▼
Static Website
```

---

# Steps

Build (optional if using a custom Dockerfile):

```bash
docker build -t static-site:v1 .
```

Run:

```bash
docker run -d \
-p 8080:80 \
--name static-site \
static-site:v1
```

Verify:

```bash
docker ps
```

Visit:

```text
http://localhost:8080
```

---

# Skills Learned

- Docker Images
- Dockerfile
- Port Mapping
- Containers

---

# Best Practices

- Use version tags.
- Keep images lightweight.
- Remove unused containers.

---

# Summary

This project demonstrates packaging and deploying a static website using Docker and Nginx.