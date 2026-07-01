# Docker Networks

## Overview

Docker Networking enables communication between containers, the host machine, and external networks. By default, every Docker container is isolated. Docker networking provides a secure and efficient way for containers to communicate with each other and with external services.

Networking is one of Docker's most important features and is widely used in microservices, cloud-native applications, and DevOps environments.

---

# Why Docker Networking?

Without networking:

- Containers cannot communicate with each other.
- Applications cannot access external services.
- Users cannot access applications running inside containers.

Docker networking solves these problems by providing:

- Container-to-container communication
- Container-to-host communication
- Internet access
- Network isolation
- Automatic IP address management
- DNS-based service discovery

---

# Docker Networking Architecture

```text
                   Internet
                       │
                       ▼
                Host Machine
                       │
         ┌─────────────┴─────────────┐
         │                           │
         ▼                           ▼
 Docker Bridge Network        Host Network
         │                           │
 ┌───────┴────────┐                  │
 │                │                  │
 ▼                ▼                  ▼
Container A   Container B      Container C
```

Each container can be connected to one or more Docker networks.

---

# Types of Docker Networks

Docker provides several built-in network drivers.

| Network Driver | Purpose |
|----------------|---------|
| Bridge | Default network for containers on a single host |
| Host | Shares the host's network stack |
| None | Disables networking completely |
| Overlay | Connects containers across multiple Docker hosts |
| Macvlan | Assigns a physical MAC address to containers |

The most commonly used drivers are **Bridge** and **Host**.

---

# Default Networks

After installing Docker, several default networks are created.

View them:

```bash
docker network ls
```

Example output:

```text
NETWORK ID     NAME      DRIVER
abcd1234       bridge    bridge
efgh5678       host      host
ijkl9012       none      null
```

---

# Bridge Network

The Bridge Network is the default Docker network.

Containers connected to the bridge network:

- Receive their own IP addresses.
- Can communicate with each other.
- Remain isolated from the host.

Example:

```bash
docker run -d nginx
```

Docker automatically connects the container to the default bridge network.

---

# Host Network

Host networking allows a container to share the host machine's network.

Example:

```bash
docker run --network host nginx
```

Characteristics:

- No separate container IP address.
- No port mapping required.
- Maximum networking performance.
- Available primarily on Linux.

---

# None Network

Containers connected to the **none** network have no network connectivity.

Example:

```bash
docker run --network none ubuntu
```

The container cannot communicate with:

- Other containers
- The host
- The Internet

This mode is useful for highly isolated workloads.

---

# Create a Custom Network

Create a new bridge network.

```bash
docker network create app-network
```

Verify.

```bash
docker network ls
```

---

# Run Containers on a Network

Run an Nginx container.

```bash
docker run -d \
--name web \
--network app-network \
nginx
```

Run an Ubuntu container.

```bash
docker run -dit \
--name client \
--network app-network \
ubuntu
```

Both containers can now communicate with each other.

---

# Inspect a Network

View detailed network information.

```bash
docker network inspect app-network
```

Example output:

```text
Name: app-network
Driver: bridge
Subnet: 172.18.0.0/16
Gateway: 172.18.0.1
```

---

# Connect a Running Container

Attach a running container to another network.

```bash
docker network connect app-network web
```

Verify.

```bash
docker network inspect app-network
```

---

# Disconnect a Container

Remove a container from a network.

```bash
docker network disconnect app-network web
```

---

# Remove a Network

Delete a Docker network.

```bash
docker network rm app-network
```

The network must not have any connected containers.

---

# DNS-Based Communication

Containers on a user-defined bridge network can communicate using container names instead of IP addresses.

Example:

```bash
ping web
```

Docker automatically resolves the container name.

This makes applications easier to configure and maintain.

---

# Docker Network Lifecycle

```text
Create Network
       │
       ▼
Run Container
       │
       ▼
Connect Container
       │
       ▼
Container Communication
       │
       ▼
Disconnect Container
       │
       ▼
Remove Network
```

---

# Common Docker Network Commands

| Command | Description |
|----------|-------------|
| `docker network ls` | List Docker networks |
| `docker network create app-network` | Create a network |
| `docker network inspect app-network` | View network details |
| `docker network connect app-network container` | Connect a container |
| `docker network disconnect app-network container` | Disconnect a container |
| `docker network rm app-network` | Remove a network |

---

# Best Practices

- Use user-defined bridge networks instead of the default bridge.
- Use meaningful network names.
- Use container names instead of IP addresses.
- Expose only the ports required by your application.
- Remove unused Docker networks regularly.
- Use Host Network only when maximum performance is required.
- Use the None Network for isolated workloads.

---

# Common Mistakes

## Using IP Addresses

Avoid configuring applications with container IP addresses.

Instead, use container names.

Example:

```text
web
```

instead of

```text
172.18.0.2
```

---

## Using the Default Bridge for Large Projects

Create custom bridge networks for better isolation and automatic DNS.

```bash
docker network create app-network
```

---

## Forgetting to Remove Unused Networks

Unused networks consume system resources.

Remove them when no longer needed.

```bash
docker network prune
```

---

## Using Host Networking Unnecessarily

Host networking reduces network isolation.

Use Bridge Networks unless high-performance networking is required.

---

# Summary

Docker Networking enables containers to communicate securely with each other, the host machine, and external services. Docker supports multiple network drivers, including Bridge, Host, None, Overlay, and Macvlan, each designed for different use cases. Understanding Docker networking is essential for building scalable, secure, and production-ready containerized applications and is a core skill for every DevOps engineer.