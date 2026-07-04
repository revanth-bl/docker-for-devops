# Docker Secrets

## Overview

Docker Secrets provide a secure method for storing and managing sensitive information such as passwords, API keys, certificates, and tokens.

Secrets are encrypted and made available only to authorized services.

---

# Why Use Secrets?

Avoid storing:

- Database passwords
- API keys
- SSL certificates
- Tokens

inside:

- Dockerfiles
- Images
- Git repositories

---

# Secret Workflow

```text
Create Secret
      │
      ▼
Store Securely
      │
      ▼
Attach to Service
      │
      ▼
Application Reads Secret
```

---

# Create a Secret

```bash
echo "mypassword" | docker secret create db_password -
```

---

# List Secrets

```bash
docker secret ls
```

---

# Inspect

```bash
docker secret inspect db_password
```

---

# Remove

```bash
docker secret rm db_password
```

---

# Using .env (Development)

```text
DB_PASSWORD=password
API_KEY=123456
```

In Compose:

```yaml
environment:
  DB_PASSWORD: ${DB_PASSWORD}
```

For production, Docker Secrets are preferred over `.env` files.

---

# Best Practices

- Never commit secrets to Git.
- Rotate secrets regularly.
- Use least privilege.
- Encrypt sensitive data.
- Store production secrets outside source code.

---

# Common Mistakes

- Hardcoding passwords.
- Uploading `.env` files to GitHub.
- Sharing secrets through images.

---

# Summary

Docker Secrets provide secure management of sensitive information, reducing the risk of exposing credentials while improving the security of containerized applications.