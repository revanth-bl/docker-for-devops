# Private Docker Registry

## Overview

A Private Docker Registry stores Docker images within an organization's infrastructure instead of using a public registry like Docker Hub.

Private registries improve security, privacy, and control over container images.

---

# Why Private Registries?

- Store proprietary images.
- Improve security.
- Faster image downloads.
- Internal deployments.
- Compliance requirements.

---

# Architecture

```text
Developer
     │
docker push
     │
     ▼
Private Registry
     │
     ▼
Production Servers
```

---

# Run a Local Registry

```bash
docker run -d \
-p 5000:5000 \
--name registry \
registry:2
```

---

# Verify

```bash
docker ps
```

Open:

```text
http://localhost:5000/v2/
```

Expected:

```text
{}
```

---

# Tag an Image

```bash
docker tag nginx localhost:5000/nginx:v1
```

---

# Push

```bash
docker push localhost:5000/nginx:v1
```

---

# Pull

```bash
docker pull localhost:5000/nginx:v1
```

---

# List Images

```bash
curl http://localhost:5000/v2/_catalog
```

---

# Best Practices

- Enable HTTPS.
- Require authentication.
- Regular backups.
- Scan images.
- Remove unused images.

---

# Common Mistakes

- Running without TLS.
- Forgetting authentication.
- Poor image naming.

---

# Summary

A private Docker registry provides secure, centralized storage for container images and is widely used in enterprise DevOps environments.