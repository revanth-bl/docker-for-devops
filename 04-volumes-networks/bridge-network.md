# Bridge Network

## Overview

The **Bridge Network** is Docker's default networking driver. It allows containers running on the same Docker host to communicate with each other while remaining isolated from the host system and external networks.

Whenever you install Docker, a default bridge network named **bridge** is automatically created. Unless another network is specified, all containers connect to this network by default.

Bridge networking is commonly used for:

- Web applications
- Development environments
- Multi-container applications
- Local testing
- Microservices running on a single Docker host

---

# Why Use a Bridge Network?

Bridge networking provides:

- Container-to-container communication
- Network isolation
- Automatic IP address assignment
- DNS-based container name resolution (on user-defined bridge networks)
- Secure communication within the Docker host

---

# How Bridge Networking Works

```text
                 Docker Host
+-------------------------------------------------+
|                                                 |
|        Docker Bridge Network (bridge)           |
|                                                 |
|  +-----------+        +-----------+             |
|  | Container |<------>| Container |             |
|  |   Web     |        | Database  |             |
|  +-----------+        +-----------+             |
|         |                     |                 |
|         +---------------------+                 |
|                                                 |
+-------------------------------------------------+
                    |
                    |
              Host Network
                    |
                Internet
```

Each container receives its own IP address and communicates through the bridge network.

---

# List Docker Networks

Display all available networks.

```bash
docker network ls
```

Example output:

```text
NETWORK ID     NAME      DRIVER    SCOPE
abcd1234       bridge    bridge    local
efgh5678       host      host      local
ijkl9012       none      null      local
```

---

# Inspect the Bridge Network

View detailed information.

```bash
docker network inspect bridge
```

Example output:

```text
Name: bridge
Driver: bridge
Subnet: 172.17.0.0/16
Gateway: 172.17.0.1
```

Docker automatically assigns IP addresses within this subnet.

---

# Run a Container on the Bridge Network

```bash
docker run -d --name web nginx
```

Verify:

```bash
docker network inspect bridge
```

The container will appear under the list of connected containers.

---

# Create a Custom Bridge Network

User-defined bridge networks are recommended because they provide automatic DNS resolution.

Create the network.

```bash
docker network create my-network
```

Verify.

```bash
docker network ls
```

---

# Run Containers on a Custom Bridge Network

Start the first container.

```bash
docker run -d \
--name web \
--network my-network \
nginx
```

Start another container.

```bash
docker run -dit \
--name ubuntu \
--network my-network \
ubuntu
```

Both containers now belong to the same network.

---

# Verify Communication

Access the Ubuntu container.

```bash
docker exec -it ubuntu bash
```

Install ping if needed.

```bash
apt update

apt install -y iputils-ping
```

Ping the Nginx container by its name.

```bash
ping web
```

Example output:

```text
PING web (172.18.0.2)
64 bytes from web
```

This works because Docker's internal DNS resolves container names on user-defined bridge networks.

---

# Connect an Existing Container

Connect a running container to a network.

```bash
docker network connect my-network web
```

Verify.

```bash
docker network inspect my-network
```

---

# Disconnect a Container

Remove a container from a network.

```bash
docker network disconnect my-network web
```

---

# Remove a Bridge Network

Delete the network.

```bash
docker network rm my-network
```

The network must not have any connected containers.

---

# Default Bridge vs User-Defined Bridge

| Default Bridge | User-Defined Bridge |
|----------------|---------------------|
| Created automatically | Created manually |
| Basic networking | Advanced networking |
| Limited DNS support | Automatic container name resolution |
| Less flexible | More configurable |
| Suitable for simple containers | Recommended for production and multi-container applications |

---

# Practical Example

Create a custom bridge network.

```bash
docker network create app-network
```

Run an Nginx container.

```bash
docker run -d \
--name web \
--network app-network \
-p 8080:80 \
nginx
```

Run an Ubuntu container.

```bash
docker run -dit \
--name client \
--network app-network \
ubuntu
```

Verify the network.

```bash
docker network inspect app-network
```

Open the browser.

```text
http://localhost:8080
```

The Nginx web server should be accessible.

---

# Common Bridge Network Commands

| Command | Description |
|----------|-------------|
| `docker network ls` | List all networks |
| `docker network inspect bridge` | Inspect the default bridge network |
| `docker network create my-network` | Create a custom bridge network |
| `docker run --network my-network nginx` | Run a container on a network |
| `docker network connect my-network web` | Connect a running container |
| `docker network disconnect my-network web` | Disconnect a container |
| `docker network rm my-network` | Remove a network |

---

# Best Practices

- Use user-defined bridge networks instead of the default bridge.
- Assign meaningful network names.
- Group related containers on the same network.
- Expose only the ports required by the application.
- Remove unused Docker networks regularly.
- Use container names instead of IP addresses for communication.

---

# Common Mistakes

## Using the Default Bridge for Large Projects

The default bridge network lacks automatic container name resolution.

Instead:

```bash
docker network create app-network
```

---

## Using IP Addresses

Avoid:

```text
172.18.0.2
```

Instead, use:

```text
web
```

Container names are easier to manage and remain consistent.

---

## Forgetting to Specify the Network

Incorrect:

```bash
docker run nginx
```

Correct:

```bash
docker run --network app-network nginx
```

---

## Removing a Network in Use

Docker will not remove a network while containers are attached.

Disconnect or remove the containers first.

---

# Summary

The Bridge Network is Docker's default networking driver and enables communication between containers on the same Docker host. While the default bridge network is suitable for simple use cases, user-defined bridge networks provide better flexibility, automatic DNS resolution, and improved container management. Understanding bridge networking is essential for building multi-container applications and is a fundamental skill for modern DevOps engineers.