# Docker Login

## Overview

`docker login` authenticates your local Docker client with a container registry such as Docker Hub.

After logging in, Docker can push images to repositories and pull private images.

---

# Login

```bash
docker login
```

Example:

```text
Username: johndoe
Password:
Login Succeeded
```

---

# Verify Login

```bash
cat ~/.docker/config.json
```

Windows:

```text
C:\Users\<username>\.docker\config.json
```

---

# Logout

```bash
docker logout
```

---

# Best Practices

- Use Docker Credential Helpers.
- Never share credentials.
- Logout from shared computers.

---

# Common Mistakes

- Incorrect username.
- Using email instead of Docker Hub username.
- Invalid Personal Access Token.

---

# Summary

`docker login` securely authenticates Docker with a registry, allowing image uploads and access to private repositories.