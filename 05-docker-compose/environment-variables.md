# Environment Variables in Docker Compose

## Overview

Environment variables are used to provide configuration values to Docker containers without modifying the application code. They allow developers to configure applications for different environments such as development, testing, and production.

Docker Compose supports defining environment variables directly inside the `compose.yaml` file or by using an external `.env` file.

Using environment variables improves flexibility, security, and portability.

---

# Why Use Environment Variables?

Without environment variables:

- Configuration is hardcoded.
- Separate Docker images may be needed for different environments.
- Updating credentials requires rebuilding containers.
- Managing multiple deployments becomes difficult.

With environment variables:

- Configuration is centralized.
- Applications become portable.
- Secrets can be managed separately.
- Deployment becomes easier.

---

# How Environment Variables Work

```text
          .env File
               │
               ▼
       Docker Compose
               │
               ▼
        Docker Container
               │
               ▼
         Running Application
```

Docker Compose passes the environment variables into the container during startup.

---

# Using the environment Section

The simplest way to define environment variables is with the `environment` key.

```yaml
services:

  app:

    image: nginx

    environment:
      APP_NAME: DockerDemo
      APP_ENV: development
```

---

# Environment Variable Syntax

Variables can be defined in two ways.

### Key-Value Format

```yaml
environment:
  APP_ENV: production
  PORT: 8080
```

---

### List Format

```yaml
environment:
  - APP_ENV=production
  - PORT=8080
```

Both formats are valid.

---

# Example: MySQL Container

```yaml
services:

  mysql:

    image: mysql:8.4

    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: appdb
      MYSQL_USER: appuser
      MYSQL_PASSWORD: app123
```

Docker automatically configures the database using these values.

---

# Using a .env File

Instead of hardcoding values, store them in a `.env` file.

Example:

```text
MYSQL_ROOT_PASSWORD=password
MYSQL_DATABASE=appdb
MYSQL_USER=appuser
MYSQL_PASSWORD=app123
APP_ENV=production
```

Compose file:

```yaml
services:

  mysql:

    image: mysql:8.4

    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      MYSQL_DATABASE: ${MYSQL_DATABASE}
      MYSQL_USER: ${MYSQL_USER}
      MYSQL_PASSWORD: ${MYSQL_PASSWORD}
```

Docker automatically loads variables from the `.env` file.

---

# env_file Option

You can also specify an environment file explicitly.

```yaml
services:

  app:

    image: nginx

    env_file:
      - .env
```

This imports all variables from the file.

---

# Variable Substitution

Compose supports variable substitution.

```yaml
services:

  app:

    image: nginx

    ports:
      - "${PORT}:80"
```

If `.env` contains:

```text
PORT=8080
```

Docker publishes port **8080**.

---

# Default Values

Provide a default value if the variable is missing.

```yaml
environment:
  APP_ENV: ${APP_ENV:-development}
```

If `APP_ENV` is not defined, Docker uses `development`.

---

# Complete Example

### .env

```text
APP_NAME=DockerDemo
APP_ENV=production
PORT=8080

MYSQL_ROOT_PASSWORD=password
MYSQL_DATABASE=appdb
```

---

### compose.yaml

```yaml
services:

  web:

    image: nginx

    ports:
      - "${PORT}:80"

    environment:
      APP_NAME: ${APP_NAME}
      APP_ENV: ${APP_ENV}

  database:

    image: mysql:8.4

    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      MYSQL_DATABASE: ${MYSQL_DATABASE}
```

Run the application.

```bash
docker compose up -d
```

---

# Verify Environment Variables

View running containers.

```bash
docker compose ps
```

Access a container.

```bash
docker exec -it web sh
```

Display variables.

```bash
printenv
```

or

```bash
env
```

Example output:

```text
APP_NAME=DockerDemo
APP_ENV=production
PORT=8080
```

---

# Common Commands

| Command | Description |
|----------|-------------|
| `docker compose up -d` | Start containers |
| `docker compose down` | Stop containers |
| `docker compose ps` | View running services |
| `docker compose config` | Validate the Compose file |
| `docker exec -it container-name sh` | Open a shell |
| `printenv` | Display environment variables |
| `env` | List environment variables |

---

# Best Practices

- Store sensitive values in a `.env` file.
- Never commit secrets to GitHub.
- Use meaningful variable names.
- Use default values where appropriate.
- Keep development and production configurations separate.
- Use `.gitignore` to exclude `.env` files containing secrets.
- Document required environment variables in your project's README.

---

# Common Mistakes

## Hardcoding Passwords

Avoid:

```yaml
environment:
  MYSQL_ROOT_PASSWORD: admin123
```

Instead:

```yaml
environment:
  MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
```

---

## Committing the .env File

Do not upload files containing passwords or API keys.

Add to `.gitignore`:

```text
.env
```

---

## Incorrect Variable Syntax

Incorrect:

```yaml
PORT=$PORT
```

Correct:

```yaml
PORT=${PORT}
```

---

## Missing Variables

If a required variable is missing, Docker may start the container with empty values or fail to start.

Always verify your `.env` file.

---

# Summary

Environment variables in Docker Compose provide a flexible way to configure applications without modifying source code. They can be defined directly in the `compose.yaml` file or loaded from an external `.env` file. Using environment variables improves portability, simplifies deployment, and helps keep sensitive configuration separate from application code, making them an essential part of modern DevOps workflows.