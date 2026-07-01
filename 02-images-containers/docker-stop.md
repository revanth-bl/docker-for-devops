# Docker Stop

## Overview

The `docker stop` command is used to gracefully stop one or more running Docker containers. Unlike forcefully terminating a container, `docker stop` allows the application inside the container to shut down properly before the container is stopped.

This command is commonly used when updating applications, performing maintenance, or safely shutting down services.

---

# Why Use Docker Stop?

Stopping a container gracefully ensures that:

* Running processes finish correctly.
* Data is written to disk before shutdown.
* Databases close active connections safely.
* Applications have time to save their current state.
* The risk of data corruption is minimized.

For these reasons, `docker stop` is recommended over `docker kill` whenever possible.

---

# Docker Stop Syntax

```bash
docker stop [OPTIONS] CONTAINER [CONTAINER...]
```

You can specify either the container name or its ID.

---

# Stop a Running Container

First, check the running containers.

```bash
docker ps
```

Example output:

```text
CONTAINER ID   IMAGE   STATUS       NAMES
a12b34c56d78   nginx   Up 5 minutes my-nginx
```

Stop the container.

```bash
docker stop my-nginx
```

Example output:

```text
my-nginx
```

Verify that it has stopped.

```bash
docker ps
```

The container will no longer appear because `docker ps` only displays running containers.

To view stopped containers:

```bash
docker ps -a
```

---

# Stop Multiple Containers

You can stop multiple containers with a single command.

```bash
docker stop container1 container2 container3
```

Docker stops each container one by one.

---

# Stop a Container Using Its ID

Instead of using the container name, you can use the container ID.

```bash
docker stop a12b34c56d78
```

Container IDs are useful when working with automation scripts.

---

# Graceful Shutdown

When you execute:

```bash
docker stop my-nginx
```

Docker sends a **SIGTERM** signal to the application's main process.

The application is given time to shut down properly.

If it does not exit within the default timeout (10 seconds on Linux), Docker sends a **SIGKILL** signal to forcefully terminate it.

Shutdown process:

```text
Running
    │
docker stop
    │
    ▼
SIGTERM
    │
Graceful Shutdown
    │
    ▼
Container Stopped

If timeout expires:

SIGKILL
    │
    ▼
Force Stop
```

---

# Change the Stop Timeout

By default, Docker waits 10 seconds before forcefully stopping the container.

Specify a custom timeout:

```bash
docker stop -t 30 my-nginx
```

Docker waits up to **30 seconds** before sending a force kill signal.

---

# Start the Container Again

Stopping a container does not remove it.

Restart it using:

```bash
docker start my-nginx
```

Verify:

```bash
docker ps
```

The container should appear as running again.

---

# Restart a Container

Instead of stopping and starting separately, restart the container.

```bash
docker restart my-nginx
```

This command stops the container and immediately starts it again.

---

# Docker Stop vs Docker Kill

Many beginners confuse these commands.

| Docker Stop                         | Docker Kill                                |
| ----------------------------------- | ------------------------------------------ |
| Gracefully stops a container        | Immediately terminates a container         |
| Sends SIGTERM first                 | Sends SIGKILL directly                     |
| Allows applications to close safely | Does not allow graceful shutdown           |
| Recommended for normal operations   | Used only when a container is unresponsive |

Example:

Graceful stop:

```bash
docker stop my-nginx
```

Force stop:

```bash
docker kill my-nginx
```

---

# Practical Example

Create an Nginx container.

```bash
docker run -d --name my-nginx -p 8080:80 nginx
```

Verify that it is running.

```bash
docker ps
```

Stop the container.

```bash
docker stop my-nginx
```

Verify that it has stopped.

```bash
docker ps
```

Display all containers.

```bash
docker ps -a
```

Restart the container.

```bash
docker start my-nginx
```

Confirm that it is running again.

```bash
docker ps
```

---

# Common Docker Stop Commands

| Command                             | Description                       |
| ----------------------------------- | --------------------------------- |
| `docker stop my-nginx`              | Stop a running container          |
| `docker stop container1 container2` | Stop multiple containers          |
| `docker stop -t 30 my-nginx`        | Wait 30 seconds before force stop |
| `docker start my-nginx`             | Start a stopped container         |
| `docker restart my-nginx`           | Restart a container               |
| `docker kill my-nginx`              | Forcefully terminate a container  |

---

# Best Practices

* Use `docker stop` instead of `docker kill` whenever possible.
* Verify running containers using `docker ps` before stopping them.
* Use meaningful container names.
* Allow sufficient shutdown time for databases and enterprise applications.
* Restart services using `docker restart` when appropriate.

---

# Common Mistakes

## Trying to Stop a Container That Is Already Stopped

If the container is already stopped, Docker returns an error.

Verify container status first.

```bash
docker ps -a
```

---

## Confusing Stop with Remove

Stopping a container does **not** delete it.

```bash
docker stop my-nginx
```

The container still exists.

To remove it:

```bash
docker rm my-nginx
```

---

## Using Docker Kill Unnecessarily

Avoid using:

```bash
docker kill my-nginx
```

unless the container is frozen or not responding.

Prefer:

```bash
docker stop my-nginx
```

for normal shutdown operations.

---

# Summary

The `docker stop` command is used to gracefully stop running Docker containers. It sends a termination signal to the application's main process, allowing it to shut down safely before the container stops. Understanding the difference between `docker stop`, `docker start`, `docker restart`, and `docker kill` is essential for effective Docker container management and is a fundamental skill for DevOps engineers.
