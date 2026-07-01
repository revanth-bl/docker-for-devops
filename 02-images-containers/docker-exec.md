# Docker Exec

## Overview

The `docker exec` command allows you to run commands inside an already running Docker container. It is one of the most frequently used Docker commands because it enables administrators and developers to inspect, troubleshoot, configure, and manage running containers without restarting them.

Instead of creating a new container, `docker exec` opens a new process inside an existing container.

---

# Why Use Docker Exec?

Suppose you have a web server, database, or application running inside a container. You may need to:

* Check application logs
* Edit configuration files
* Install debugging tools
* View running processes
* Test network connectivity
* Verify application status

Without `docker exec`, you would have to stop the container or rebuild the image. Using `docker exec`, you can perform these tasks while the container continues running.

---

# Docker Exec Syntax

```bash
docker exec [OPTIONS] CONTAINER COMMAND
```

### Example

```bash
docker exec my-nginx ls
```

This runs the `ls` command inside the `my-nginx` container.

---

# Docker Run vs Docker Exec

Many beginners confuse these two commands.

| Docker Run               | Docker Exec                               |
| ------------------------ | ----------------------------------------- |
| Creates a new container  | Uses an existing running container        |
| Starts a new application | Runs a command inside a running container |
| Requires an image        | Requires a running container              |
| Used for deployment      | Used for administration and debugging     |

Example:

Create a new container:

```bash
docker run -d --name my-nginx nginx
```

Run a command inside that container:

```bash
docker exec my-nginx ls
```

Notice that the second command does **not** create another container.

---

# Common Docker Exec Commands

## List Files

```bash
docker exec my-nginx ls
```

Lists files in the current working directory.

---

## List Files with Details

```bash
docker exec my-nginx ls -l
```

Displays detailed file information.

---

## Print Working Directory

```bash
docker exec my-nginx pwd
```

Shows the current directory inside the container.

---

## Display Operating System Information

```bash
docker exec my-nginx cat /etc/os-release
```

Displays details about the Linux distribution running inside the container.

Example output:

```text
PRETTY_NAME="Debian GNU/Linux"
VERSION="12 (bookworm)"
```

---

## View Running Processes

```bash
docker exec my-nginx ps -ef
```

Shows all running processes inside the container.

---

## Check Current User

```bash
docker exec my-nginx whoami
```

Displays the user executing commands inside the container.

---

## Check Network Configuration

```bash
docker exec my-nginx ip addr
```

Displays network interface information.

---

## Display Environment Variables

```bash
docker exec my-nginx env
```

Lists all environment variables available inside the container.

---

# Interactive Mode (-it)

One of the most common uses of `docker exec` is opening an interactive shell.

```bash
docker exec -it my-nginx bash
```

Options explained:

* `-i` → Interactive mode (keeps STDIN open)
* `-t` → Allocates a terminal

You now have direct access to the container's shell.

Example:

```text
root@4d3b9f5:/#
```

From here you can execute Linux commands just as if you were logged into a normal server.

---

# If Bash Is Not Available

Some lightweight images (such as Alpine Linux) do not include Bash.

Instead, use:

```bash
docker exec -it alpine-container sh
```

Always use the shell that exists inside the container.

---

# Execute Multiple Commands

You can execute multiple commands together.

```bash
docker exec my-nginx sh -c "pwd && ls -l"
```

Output:

```text
/
total 8
drwxr-xr-x 2 root root 4096
```

---

# Running Commands as Another User

Use the `-u` option.

```bash
docker exec -u root my-nginx whoami
```

Output:

```text
root
```

You can also specify another user if it exists inside the container.

---

# Practical Example

Create an Nginx container.

```bash
docker run -d --name my-nginx -p 8080:80 nginx
```

Verify it is running.

```bash
docker ps
```

Open an interactive shell.

```bash
docker exec -it my-nginx bash
```

Inside the container:

```bash
pwd
ls
cd /usr/share/nginx/html
ls
cat index.html
exit
```

After exiting, the container continues running.

---

# Common Docker Exec Options

| Option | Description               |
| ------ | ------------------------- |
| `-i`   | Interactive mode          |
| `-t`   | Allocate terminal         |
| `-u`   | Execute as specific user  |
| `-e`   | Set environment variable  |
| `-w`   | Specify working directory |

Example:

```bash
docker exec -w /usr/share/nginx/html my-nginx pwd
```

---

# Best Practices

* Use `docker exec` only on running containers.
* Prefer interactive mode for troubleshooting.
* Exit the shell after completing your work.
* Avoid making permanent configuration changes inside containers; update the Dockerfile instead.
* Use Docker volumes for persistent data.
* Minimize manual changes inside production containers.

---

# Common Mistakes

### Trying to use docker exec on a stopped container

```bash
docker exec my-nginx bash
```

Error:

```text
Error response from daemon: Container is not running
```

Start the container first.

```bash
docker start my-nginx
```

---

### Confusing docker run with docker exec

Remember:

* `docker run` creates a new container.
* `docker exec` runs commands inside an existing container.

---

### Assuming Bash Exists

Not every container includes Bash.

If you receive:

```text
bash: command not found
```

Use:

```bash
docker exec -it container-name sh
```

---

# Summary

The `docker exec` command is an essential tool for interacting with running Docker containers. It allows developers to inspect files, execute commands, troubleshoot issues, and manage applications without creating new containers or interrupting running services. Mastering `docker exec` is a fundamental skill for Docker administration and everyday DevOps workflows.

---

