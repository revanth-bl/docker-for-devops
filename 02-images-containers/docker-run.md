# Docker Run

## Overview

The `docker run` command is one of the most important Docker commands. It is used to create and start a new container from a Docker image.

When you execute `docker run`, Docker performs several operations automatically:

1. Checks whether the image exists locally.
2. Downloads the image from Docker Hub if it is not available.
3. Creates a new container.
4. Starts the container.
5. Executes the default command defined in the image.

The `docker run` command is used daily by developers and DevOps engineers to deploy applications, test software, and run services inside isolated environments.

---

# Docker Run Syntax

```bash
docker run [OPTIONS] IMAGE [COMMAND]
```

Example:

```bash
docker run nginx
```

Docker creates and starts a new container using the **nginx** image.

---

# Running Your First Container

Run an Nginx container.

```bash
docker run nginx
```

If the image does not exist locally, Docker downloads it automatically.

Example output:

```text
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
...
Status: Downloaded newer image for nginx:latest
```

---

# Run a Container in Detached Mode

Normally, Docker attaches your terminal to the container.

To run the container in the background, use the `-d` option.

```bash
docker run -d nginx
```

Example output:

```text
4d9b85c9b8c0d9ef6e...
```

Verify it is running.

```bash
docker ps
```

Detached mode is commonly used for web servers, databases, and production services.

---

# Assign a Custom Container Name

Docker automatically generates random names if one is not provided.

Assign your own name:

```bash
docker run -d --name my-nginx nginx
```

Verify:

```bash
docker ps
```

Example:

```text
CONTAINER ID   IMAGE   STATUS      NAMES
xxxxxxxxxxxx   nginx   Up 2 mins   my-nginx
```

---

# Publish Container Ports

Containers have their own network.

To access a service from your computer, publish its ports.

```bash
docker run -d -p 8080:80 --name my-nginx nginx
```

Explanation:

* **8080** → Host port
* **80** → Container port

Open your browser:

```text
http://localhost:8080
```

You should see the default Nginx welcome page.

---

# Interactive Mode

Some containers are used interactively.

Example:

```bash
docker run -it ubuntu
```

Options:

* `-i` → Interactive mode
* `-t` → Allocate a terminal

You will enter the Ubuntu shell directly.

Example:

```text
root@container:/#
```

Exit the shell:

```bash
exit
```

---

# Automatically Remove a Container

Temporary containers can be deleted automatically after they stop.

```bash
docker run --rm ubuntu echo "Hello Docker"
```

After execution, Docker removes the container automatically.

---

# Run a Specific Image Version

Instead of using the latest version, specify a tag.

```bash
docker run nginx:1.27
```

Using version tags improves consistency across environments.

---

# Set Environment Variables

Applications often require environment variables.

```bash
docker run -e APP_ENV=production nginx
```

Multiple variables can be specified.

```bash
docker run \
-e USER=admin \
-e PASSWORD=secret \
nginx
```

---

# Mount a Volume

Persist container data using Docker volumes.

```bash
docker run -d \
-v myvolume:/usr/share/nginx/html \
nginx
```

Volumes prevent data loss when containers are removed.

---

# Connect to a Docker Network

Specify a custom network.

```bash
docker run --network bridge nginx
```

Or:

```bash
docker run --network my-network nginx
```

This allows containers to communicate with each other.

---

# Limit System Resources

Limit memory usage.

```bash
docker run -m 512m nginx
```

Limit CPU usage.

```bash
docker run --cpus="1.5" nginx
```

Resource limits help prevent containers from consuming excessive system resources.

---

# Practical Example

Download the image.

```bash
docker pull nginx
```

Run the container.

```bash
docker run -d \
--name my-nginx \
-p 8080:80 \
nginx
```

Verify:

```bash
docker ps
```

Open:

```text
http://localhost:8080
```

Stop the container.

```bash
docker stop my-nginx
```

Remove it.

```bash
docker rm my-nginx
```

---

# Common Docker Run Options

| Option      | Description                               |
| ----------- | ----------------------------------------- |
| `-d`        | Run container in detached mode            |
| `-it`       | Interactive terminal                      |
| `--name`    | Assign a container name                   |
| `-p`        | Publish ports                             |
| `-e`        | Set environment variables                 |
| `-v`        | Mount volumes                             |
| `--rm`      | Remove container automatically after exit |
| `--network` | Connect to a network                      |
| `-m`        | Limit memory usage                        |
| `--cpus`    | Limit CPU usage                           |

---

# Best Practices

* Use meaningful container names.
* Use specific image versions instead of `latest` in production.
* Run services in detached mode.
* Publish only the ports that are required.
* Store persistent data in Docker volumes.
* Set resource limits for production workloads.
* Remove unused containers regularly.

---

# Common Mistakes

## Forgetting Detached Mode

Running:

```bash
docker run nginx
```

attaches your terminal to the container.

Instead:

```bash
docker run -d nginx
```

---

## Port Already in Use

If you receive:

```text
Bind for 0.0.0.0:8080 failed
```

Another application is already using port **8080**.

Use another port.

```bash
docker run -d -p 9090:80 nginx
```

---

## Using Random Container Names

Instead of:

```text
adoring_turing
```

Use:

```bash
docker run --name web-server nginx
```

Meaningful names simplify management.

---

## Using the Latest Tag in Production

Avoid:

```bash
docker run nginx:latest
```

Prefer:

```bash
docker run nginx:1.27
```

Specific versions ensure predictable deployments.

---

# Summary

The `docker run` command is the primary way to create and start Docker containers. It supports numerous options for networking, storage, environment variables, resource limits, and container management. Understanding `docker run` is essential for working with Docker in development, testing, and production environments.
