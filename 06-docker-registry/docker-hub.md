# Docker Hub

## Overview

Docker Hub is Docker's official cloud-based container registry. It allows developers to store, manage, share, and distribute Docker images.

Docker Hub contains millions of public images and also supports private repositories for secure image storage.

It is the default registry used by Docker when pulling or pushing images.

---

# Why Docker Hub?

Docker Hub provides:

- Public image repositories
- Private repositories
- Image versioning
- Team collaboration
- Automated builds
- Easy image sharing

---

# Docker Hub Architecture

```text
Developer
     │
     ▼
Docker CLI
     │
     ▼
 Docker Hub
     │
     ▼
Image Repository
```

---

# Create a Docker Hub Account

1. Visit https://hub.docker.com
2. Create an account.
3. Verify your email.

---

# Search Images

```bash
docker search nginx
```

---

# Pull an Image

```bash
docker pull nginx
```

---

# View Local Images

```bash
docker images
```

---

# Common Docker Hub Commands

| Command | Description |
|----------|-------------|
| `docker search nginx` | Search images |
| `docker pull nginx` | Download image |
| `docker login` | Login |
| `docker logout` | Logout |
| `docker push image` | Upload image |

---

# Best Practices

- Use verified images.
- Use official images when possible.
- Pin image versions.
- Keep repositories organized.

---

# Common Mistakes

- Using untrusted images.
- Always using `latest`.
- Forgetting to logout on shared systems.

---

# Summary

Docker Hub is the official Docker image registry used to store, distribute, and manage container images for development and production environments.