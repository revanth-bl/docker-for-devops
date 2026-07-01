# Docker PS

## Overview

The `docker ps` command is used to display information about Docker containers. It allows you to view running containers, inspect their status, check exposed ports, and monitor container activity.

As a DevOps engineer, `docker ps` is one of the most frequently used commands because it provides a quick overview of all active containers on a system.

---

# Docker PS Syntax

```bash
docker ps [OPTIONS]
```

Running the command without any options displays only the containers that are currently running.

```bash
docker ps
```

---

# Understanding the Output

Example:

```bash
docker ps
```

Output:

```text
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                  NAMES
8d7ab29c2d4e   nginx     "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes   0.0.0.0:8080->80/tcp   my-nginx
```

### Explanation of Each Column

| Column           | Description                                             |
| ---------------- | ------------------------------------------------------- |
| **CONTAINER ID** | Unique identifier assigned to the container.            |
| **IMAGE**        | Docker image used to create the container.              |
| **COMMAND**      | The command executed when the container started.        |
| **CREATED**      | Time since the container was created.                   |
| **STATUS**       | Current state of the container (Running, Exited, etc.). |
| **PORTS**        | Port mappings between the host and the container.       |
| **NAMES**        | Human-readable name assigned to the container.          |

---

# Listing Running Containers

Display all running containers.

```bash
docker ps
```

Example output:

```text
CONTAINER ID   IMAGE     STATUS      PORTS                  NAMES
4a9e51b1f91d   nginx     Up 10 mins  0.0.0.0:8080->80/tcp   my-nginx
```

---

# Listing All Containers

By default, Docker only shows running containers.

To display both running and stopped containers:

```bash
docker ps -a
```

Example:

```text
CONTAINER ID   IMAGE     STATUS                      NAMES
4a9e51b1f91d   nginx     Up 10 minutes               my-nginx
98bcfe27dd61   ubuntu    Exited (0) 5 minutes ago    ubuntu-test
```

---

# Display Only Container IDs

Sometimes scripts only need container IDs.

```bash
docker ps -q
```

Example output:

```text
4a9e51b1f91d
98bcfe27dd61
```

The `-q` option stands for **quiet mode**.

---

# Display the Latest Container

Show only the most recently created container.

```bash
docker ps -l
```

---

# Show the Last Five Containers

```bash
docker ps -n 5
```

Displays information about the last five created containers.

---

# Filter Containers

Display only running Nginx containers.

```bash
docker ps --filter "ancestor=nginx"
```

Display only running containers.

```bash
docker ps --filter "status=running"
```

Display only exited containers.

```bash
docker ps --filter "status=exited"
```

Filtering is useful when working with many containers.

---

# Format the Output

Docker supports custom formatting.

```bash
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Image}}"
```

Example:

```text
NAMES        STATUS          IMAGE
my-nginx     Up 5 minutes    nginx
redis-db     Up 2 hours      redis
```

Formatting makes command output easier to read or integrate into scripts.

---

# Show Container Size

Display the writable size of each container.

```bash
docker ps --size
```

Example:

```text
CONTAINER ID   IMAGE   SIZE
4a9e51b1f91d   nginx   5B (virtual 142MB)
```

---

# Practical Example

Create a container.

```bash
docker run -d --name my-nginx -p 8080:80 nginx
```

Verify that it is running.

```bash
docker ps
```

Expected output:

```text
CONTAINER ID   IMAGE   STATUS      PORTS                  NAMES
xxxxxxxxxxxx   nginx   Up 3 secs   0.0.0.0:8080->80/tcp   my-nginx
```

Stop the container.

```bash
docker stop my-nginx
```

Run `docker ps` again.

```bash
docker ps
```

No output will appear because the container has stopped.

Display all containers.

```bash
docker ps -a
```

Now you will see the stopped container with its status.

---

# Common Docker PS Options

| Command              | Description                   |
| -------------------- | ----------------------------- |
| `docker ps`          | Show running containers       |
| `docker ps -a`       | Show all containers           |
| `docker ps -q`       | Show only container IDs       |
| `docker ps -l`       | Show the latest container     |
| `docker ps -n 5`     | Show the last five containers |
| `docker ps --size`   | Display container sizes       |
| `docker ps --format` | Customize output format       |
| `docker ps --filter` | Filter containers             |

---

# Best Practices

* Use `docker ps` before executing management commands.
* Use meaningful container names for easier identification.
* Use filters when managing multiple containers.
* Regularly remove unused containers to keep the system clean.
* Use formatted output for automation and scripting.

---

# Common Mistakes

## Forgetting Stopped Containers

Many beginners think a container has been deleted because it does not appear in `docker ps`.

Remember:

```bash
docker ps
```

Only displays running containers.

To view stopped containers:

```bash
docker ps -a
```

---

## Confusing Container ID with Image ID

`docker ps` displays **Container IDs**, not Image IDs.

Use:

```bash
docker images
```

to view Docker images.

---

## Assuming Empty Output Means Docker Is Broken

If `docker ps` returns no output, it simply means there are no running containers.

Verify by checking:

```bash
docker ps -a
```

---

# Summary

The `docker ps` command is the primary tool for monitoring Docker containers. It provides information about running containers, their status, ports, names, and other useful details. Additional options such as `-a`, `-q`, `--filter`, and `--format` make it even more powerful for troubleshooting, automation, and day-to-day container management.

---

