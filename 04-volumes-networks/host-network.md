# Host Network

## Overview

The **Host Network** is a Docker networking mode where a container shares the host machine's network stack instead of receiving its own isolated network.

Unlike the default Bridge Network, a container running in Host Network mode does **not** receive a separate IP address. Instead, it uses the host's network interfaces directly.

This networking mode provides the highest networking performance because Docker does not perform Network Address Translation (NAT) or port forwarding.

Host networking is commonly used for:

- High-performance applications
- Monitoring tools
- Network utilities
- Performance benchmarking
- Applications requiring direct access to the host network

> **Note:** Host networking is supported on Linux. It is not supported in the same way on Docker Desktop for Windows and macOS because Docker runs inside a virtual machine.

---

# Why Use Host Networking?

Host networking offers several advantages:

- Maximum network performance
- No NAT overhead
- No port mapping required
- Lower latency
- Direct access to host network interfaces

However, it sacrifices network isolation.

---

# How Host Networking Works

```text
                  Host Machine
+-----------------------------------------+
|                                         |
|      Host Network Stack                 |
|                                         |
|  +-------------------------------+      |
|  | Docker Container              |      |
|  | (--network host)              |      |
|  |                               |      |
|  | Shares Host Network           |      |
|  +-------------------------------+      |
|                                         |
+-----------------------------------------+
                 |
                 |
             Internet
```

The container uses the host's IP address instead of its own.

---

# Run a Container Using Host Networking

Use the `--network host` option.

```bash
docker run --network host nginx
```

Unlike bridge networking, there is **no need** to publish ports with `-p`.

---

# Verify the Running Container

List running containers.

```bash
docker ps
```

You will notice that the **PORTS** column is empty because Docker is not performing port mapping.

Example:

```text
CONTAINER ID   IMAGE    STATUS      PORTS
ab12cd34ef56   nginx    Up 2 mins
```

---

# Access the Application

If Nginx listens on port **80**, simply open:

```text
http://localhost
```

No port mapping such as `8080:80` is required.

---

# Host Network vs Bridge Network

| Host Network | Bridge Network |
|---------------|----------------|
| Shares host network | Uses virtual bridge |
| No separate container IP | Each container has its own IP |
| No port mapping required | Port mapping required |
| Highest performance | Slight networking overhead |
| Less secure | Better isolation |
| Linux only | Linux, Windows, and macOS |

---

# Verify Network Mode

Inspect the container.

```bash
docker inspect my-container
```

Example output:

```text
"NetworkMode": "host"
```

This confirms the container is using the host network.

---

# Practical Example

Run an Nginx container.

```bash
docker run -d \
--name host-nginx \
--network host \
nginx
```

Verify.

```bash
docker ps
```

Inspect the container.

```bash
docker inspect host-nginx
```

Open your browser.

```text
http://localhost
```

The default Nginx page should appear.

---

# Host Networking Use Cases

Host networking is commonly used for:

- Prometheus Node Exporter
- Monitoring agents
- Network analyzers
- DNS servers
- Performance testing tools
- Applications requiring low network latency

It is generally **not recommended** for standard web applications because it reduces network isolation.

---

# Common Host Network Commands

| Command | Description |
|----------|-------------|
| `docker run --network host nginx` | Run a container using the host network |
| `docker ps` | List running containers |
| `docker inspect container-name` | View network configuration |
| `docker stop container-name` | Stop the container |
| `docker rm container-name` | Remove the container |

---

# Best Practices

- Use host networking only when high network performance is required.
- Prefer Bridge Networks for most applications.
- Avoid running multiple services on the same host ports.
- Use host networking mainly on Linux servers.
- Ensure applications do not conflict with services already running on the host.
- Limit host networking to trusted containers because it reduces network isolation.

---

# Common Mistakes

## Using Port Mapping

Incorrect:

```bash
docker run -p 8080:80 --network host nginx
```

Port mapping is ignored when using Host Network mode.

Correct:

```bash
docker run --network host nginx
```

---

## Running Multiple Services on the Same Port

Since containers share the host's network, two applications cannot listen on the same port simultaneously.

Example:

```text
Host Port 80
      │
      ├── Nginx (Running)
      └── Another Nginx (Fails)
```

---

## Assuming Host Networking Works the Same Everywhere

Host networking behaves differently on Docker Desktop (Windows/macOS) because Docker runs inside a virtual machine.

It works best on native Linux installations.

---

## Using Host Networking for Every Application

Most applications work better with Bridge Networks because they provide better isolation and flexibility.

Use Host Network mode only when its performance benefits are required.

---

# Summary

Host Networking allows a Docker container to share the host machine's network stack, eliminating the need for virtual bridge networks and port mapping. This provides the best possible networking performance with lower latency but reduces network isolation. Host networking is primarily used for high-performance, monitoring, and networking applications on Linux systems, while Bridge Networks remain the preferred choice for most containerized applications.