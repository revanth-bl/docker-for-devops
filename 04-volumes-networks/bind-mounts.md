# Bind Mounts

## Overview

A **Bind Mount** is a Docker storage mechanism that allows a directory or file from the host machine to be mounted directly into a Docker container.

Unlike Docker volumes, bind mounts use an existing path on the host system. Any changes made to the files are immediately reflected in both the host and the container.

Bind mounts are commonly used during application development because developers can edit files on the host machine while the running container instantly sees those changes.

---

# Why Use Bind Mounts?

Without bind mounts:

- Files remain inside the container.
- Changes are lost when the container is removed.
- Editing source code requires rebuilding the image.

With bind mounts:

- Files remain on the host.
- Changes appear immediately inside the container.
- No need to rebuild images after every code change.
- Easier debugging and development.

---

# How Bind Mounts Work

```text
Host Machine
┌────────────────────┐
│ /home/user/project │
└──────────┬─────────┘
           │
           │ Bind Mount
           ▼
Docker Container
┌────────────────────┐
│      /app          │
└────────────────────┘
```

Both locations reference the same files.

---

# Bind Mount Syntax

```bash
docker run -v HOST_PATH:CONTAINER_PATH IMAGE
```

Example:

```bash
docker run -v $(pwd):/app nginx
```

Windows PowerShell:

```powershell
docker run -v ${PWD}:/app nginx
```

Docker mounts the current directory into `/app` inside the container.

---

# Using the --mount Option

Docker recommends using the more explicit `--mount` syntax.

```bash
docker run \
--mount type=bind,source=$(pwd),target=/app \
nginx
```

Windows PowerShell:

```powershell
docker run `
--mount type=bind,source=${PWD},target=/app `
nginx
```

---

# Example 1: Mount Current Directory

Project structure:

```text
my-app/
│
├── index.html
├── style.css
└── Dockerfile
```

Run:

```bash
docker run -d \
-p 8080:80 \
-v $(pwd):/usr/share/nginx/html \
nginx
```

Open:

```text
http://localhost:8080
```

Editing `index.html` on your computer instantly updates the website.

---

# Example 2: Mount a Specific Folder

Linux/macOS

```bash
docker run \
-v /home/user/data:/data \
ubuntu
```

Windows

```powershell
docker run `
-v C:\Users\John\data:/data `
ubuntu
```

Everything inside the host folder is accessible inside the container.

---

# Read-Only Bind Mount

Prevent the container from modifying host files.

```bash
docker run \
-v $(pwd):/app:ro \
nginx
```

or

```bash
docker run \
--mount type=bind,source=$(pwd),target=/app,readonly \
nginx
```

This is useful for serving static files securely.

---

# Verify Bind Mounts

Inspect the container.

```bash
docker inspect my-container
```

Example output:

```text
"Mounts": [
  {
    "Type": "bind",
    "Source": "/home/user/project",
    "Destination": "/app"
  }
]
```

---

# Bind Mount vs Docker Volume

| Bind Mount | Docker Volume |
|------------|---------------|
| Uses host directory | Managed by Docker |
| Easy for development | Best for production |
| Depends on host path | Portable |
| Files visible on host | Stored in Docker's managed storage |
| Faster for editing code | Better for persistent application data |

---

# Practical Example

Create a folder.

```bash
mkdir docker-bind-demo
cd docker-bind-demo
```

Create a simple web page.

```html
<h1>Hello Docker!</h1>
```

Run Nginx.

```bash
docker run -d \
--name nginx-bind \
-p 8080:80 \
-v $(pwd):/usr/share/nginx/html \
nginx
```

Verify.

```bash
docker ps
```

Open your browser.

```text
http://localhost:8080
```

Edit the HTML file.

Refresh the browser.

The changes appear immediately without restarting the container.

---

# Common Bind Mount Commands

| Command | Description |
|----------|-------------|
| `docker run -v $(pwd):/app nginx` | Mount current directory |
| `docker run --mount type=bind,source=$(pwd),target=/app nginx` | Mount using --mount |
| `docker inspect container-name` | View mount information |
| `docker exec -it container-name bash` | Verify mounted files |
| `docker stop container-name` | Stop the container |

---

# Best Practices

- Use bind mounts primarily for development.
- Prefer Docker volumes for production data.
- Use read-only mounts whenever possible.
- Avoid mounting sensitive system directories.
- Keep host paths organized and well documented.
- Use the `--mount` syntax for better readability.

---

# Common Mistakes

## Mounting the Wrong Directory

Always verify the source path.

Incorrect:

```bash
docker run -v /wrong/path:/app nginx
```

Correct:

```bash
docker run -v $(pwd):/app nginx
```

---

## Using Bind Mounts for Databases

Database files should typically use Docker volumes rather than bind mounts for better portability and reliability.

---

## Forgetting Read-Only Mode

If the container only needs to read files, use:

```bash
docker run -v $(pwd):/app:ro nginx
```

This prevents accidental modifications.

---

## Platform-Specific Paths

Linux/macOS:

```bash
$(pwd)
```

Windows PowerShell:

```powershell
${PWD}
```

Use the correct syntax for your operating system.

---

# Summary

Bind mounts allow Docker containers to directly access files and directories from the host machine. They are ideal for development because changes made on the host are immediately reflected inside the container without rebuilding the image. While bind mounts provide flexibility and convenience during development, Docker volumes are generally the preferred choice for persistent data in production environments.