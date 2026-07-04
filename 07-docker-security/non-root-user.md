# Running Containers as a Non-Root User

## Overview

By default, many Docker containers run as the root user. Running applications as a non-root user reduces the impact of security vulnerabilities and follows the principle of least privilege.

---

# Why Avoid Root?

Running as root can allow:

- Privilege escalation
- Host access
- Greater impact if compromised

---

# Dockerfile Example

```dockerfile
FROM nginx:1.27

RUN useradd -m appuser

USER appuser
```

---

# Verify User

```bash
docker exec container-name whoami
```

Output:

```text
appuser
```

---

# Check User ID

```bash
docker exec container-name id
```

---

# Workflow

```text
Create User
      │
      ▼
Assign Permissions
      │
      ▼
Switch User
      │
      ▼
Run Container
```

---

# Best Practices

- Never run production containers as root.
- Use dedicated application users.
- Grant only required permissions.
- Verify permissions regularly.

---

# Common Mistakes

- Running every container as root.
- Giving unnecessary privileges.
- Using `--privileged` without reason.

---

# Summary

Running containers as a non-root user significantly improves Docker security by reducing the damage that can occur if a container is compromised.