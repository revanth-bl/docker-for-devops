# Docker Push

## Overview

`docker push` uploads Docker images from the local machine to a container registry.

---

# Tag an Image

```bash
docker tag myapp username/myapp:v1
```

---

# Login

```bash
docker login
```

---

# Push Image

```bash
docker push username/myapp:v1
```

---

# Verify Repository

Visit Docker Hub and confirm the image appears.

---

# Workflow

```text
Dockerfile
     │
docker build
     │
docker tag
     │
docker login
     │
docker push
     │
Docker Hub
```

---

# Common Commands

```bash
docker tag

docker login

docker push
```

---

# Best Practices

- Tag images properly.
- Use semantic versioning.
- Push tested images only.

---

# Common Mistakes

- Forgetting to tag.
- Wrong repository name.
- Not logged in.

---

# Summary

`docker push` uploads locally built images to Docker Hub or another registry, making them available to other users and deployment environments.