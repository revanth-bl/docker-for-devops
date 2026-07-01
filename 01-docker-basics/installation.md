# Docker Installation

## Overview

Docker is an open-source platform that enables developers to build, package, distribute, and run applications inside lightweight, portable containers. Containers ensure applications run consistently across development, testing, and production environments.

Docker is a core technology in modern DevOps, cloud-native development, CI/CD pipelines, and microservices architectures.

---

# Learning Objectives

After reading this guide, you will be able to:

- Understand Docker prerequisites
- Install Docker Desktop
- Verify the installation
- Understand Docker components
- Troubleshoot common installation issues

---

# System Requirements

## Windows

- Windows 10/11 (64-bit)
- WSL2 Enabled
- Virtualization Enabled
- 4 GB RAM Minimum

---

## Linux

- Ubuntu 20.04+
- Debian
- CentOS
- Fedora
- RHEL

---

## macOS

- macOS 11+
- Intel or Apple Silicon

---

# Install Docker

Download Docker Desktop from:

https://www.docker.com/products/docker-desktop/

Follow the installation wizard.

---

# Verify Installation

```bash
docker --version
```

Example:

```text
Docker version 28.x.x
```

Check Docker information.

```bash
docker info
```

Run the test container.

```bash
docker run hello-world
```

Expected output:

```text
Hello from Docker!
```

---

# Verify Docker Service

```bash
docker version
```

---

# Common Installation Issues

## Docker daemon not running

Start Docker Desktop or restart the Docker service.

---

## Permission denied (Linux)

Add your user to the Docker group.

```bash
sudo usermod -aG docker $USER
```

Then log out and log back in.

---

# Best Practices

- Keep Docker updated.
- Enable WSL2 on Windows.
- Allocate sufficient CPU and memory.
- Use stable Docker releases.

---

# References

Docker Documentation

https://docs.docker.com/