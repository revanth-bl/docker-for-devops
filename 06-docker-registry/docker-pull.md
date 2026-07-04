# Docker Pull

## Overview

The `docker pull` command downloads Docker images from a container registry to the local machine.

---

# Pull Latest Image

```bash
docker pull nginx
```

---

# Pull Specific Version

```bash
docker pull nginx:1.27
```

---

# Pull Ubuntu

```bash
docker pull ubuntu:24.04
```

---

# Verify Images

```bash
docker images
```

---

# Workflow

```text
Docker Hub
      │
      ▼
docker pull
      │
      ▼
Local Images
```

---

# Best Practices

- Pull specific versions.
- Remove unused images.
- Verify image sources.

---

# Common Mistakes

- Forgetting tags.
- Pulling large unnecessary images.
- Assuming images update automatically.

---

# Summary

`docker pull` downloads container images from a registry and stores them locally for creating containers.