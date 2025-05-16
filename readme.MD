# Expense Application Deployment with Docker

This repository contains Docker configurations for setting up the Expense application. The application consists of three main components: the frontend (served by Nginx), the backend (Node.js application), and the MySQL database. These components are configured and deployed using Docker.

## Overview

The deployment process involves setting up the following:

1. **Frontend**: Configured using Nginx, serving static files.
2. **Backend**: A Node.js application, deployed and configured as a service.
3. **Database**: MySQL server setup and schema loading.

## Directory Structure
. ├── 01-mysql │ 

    ├── Dockerfile │
    
     └── scripts │ └── backend.sql 


. ├── 02-backend │

      ├── Dockerfile │ ├── package.json │ ├── index.js │ ├── DbConfig.js │ └── TransactionService.js 


.  └── 03-frontend 

    ├── Dockerfile ├── expense.conf └── code ├── index.html └── asset-manifest.json


## Prerequisites

- Docker installed on your machine.
- Docker Compose installed on your machine.

## Docker Installation Steps

```sh
cd docker-expense
sudo sh docker-install.sh
```

## Deployment Steps

### Create Network
```sh
docker network create expense
```

### Database Setup

1. MySQL setup Navigate to the `mysql` directory and build the Docker image:

```sh
cd mysql
docker build -t mysql:v1.0 .
```

Run the MySQL container:
```sh
docker run -d -p 3306:3306 --name mysql --network expense mysql:v1.0
```

2. Backend Setup
Navigate to the 02-backend directory and build the Docker image:

```sh
cd ../backend
docker build -t backend:v1.0 .
```

Run the backend container:
```sh
docker run -d -p 8080:8080 --name backend --network expense backend:v1.0
```

3. Frontend Setup
Navigate to the 03-frontend directory and build the Docker image:

```sh
cd ../frontend
docker build -t frontend:v1.0 .
```

```sh
Run the frontend container:
```sh
docker run -d -p 8080:8080 --name frontend --network expense frontend:v1.0
```



# Docker Best Practices

## 1. Use Official and Verified Images
- **Official Images:** Start with official images from Docker Hub when possible, as they are maintained and updated regularly.
- **Minimal Base Images:** Use minimal base images (like `alpine`) to reduce the attack surface and image size.

## 2. Keep Images Small
- **Multi-Stage Builds:** Use multi-stage builds to minimize the final image size by only including necessary components.
- **Remove Unnecessary Files:** Clean up package lists, cache files, and other unnecessary files to keep the image lean.

## 3. Tag Images Properly
- **Version Tagging:** Tag images with version numbers (e.g., `myapp:1.0`) instead of just using `latest` to avoid ambiguity and ensure consistency across deployments.

## 4. Manage Secrets Securely
- **Environment Variables:** Avoid hardcoding secrets in your Dockerfile or image. Use environment variables or Docker secrets for sensitive information.
- **Docker Secrets:** Utilize Docker Swarm or Kubernetes secrets management for secure storage and usage of sensitive data.

## 5. Use Dockerignore
- **.dockerignore File:** Include a `.dockerignore` file to prevent unnecessary files and directories from being added to the build context, speeding up the build process and keeping the image clean.

## 6. Run as Non-Root User
- **Non-Root User:** Create and run containers with a non-root user to improve security. You can add a user in the Dockerfile using `RUN useradd -m myuser` and switch to that user with `USER myuser`.

## 7. Optimize Layer Caching
- **Layer Order:** Order your Dockerfile instructions to maximize layer caching. Frequently changing commands should be at the end of the Dockerfile.
- **Combine Commands:** Combine commands where possible to reduce the number of layers, but balance it with readability.

## 8. Leverage Health Checks
- **Healthcheck:** Define `HEALTHCHECK` instructions in your Dockerfile to allow Docker to monitor the health of your application and take action if it becomes unhealthy.

## 9. Use Volumes for Persistent Data
- **Volumes:** Use Docker volumes to persist data outside of the container’s filesystem, ensuring data is not lost when the container is removed or updated.
- **Bind Mounts:** Use bind mounts for development to sync code changes in real-time.

## 10. Limit Container Resources
- **Resource Constraints:** Set resource limits (`--memory`, `--cpu-shares`, etc.) to prevent a single container from consuming excessive resources on the host.

## 11. Network Configuration
- **Custom Networks:** Use custom networks to isolate containers and manage communication more securely and efficiently.
- **Service Discovery:** Use Docker's built-in DNS service for container name resolution within custom networks.

## 12. Log Management
- **Log Drivers:** Configure appropriate log drivers (`json-file`, `syslog`, `fluentd`, etc.) to manage container logs effectively.
- **Centralized Logging:** Use centralized logging solutions to aggregate and analyze logs from multiple containers and hosts.

## 13. Security Best Practices
- **Minimal Privileges:** Grant the minimum necessary privileges to your containers.
- **Regular Updates:** Regularly update base images and dependencies to mitigate vulnerabilities.

## 14. Automated Builds and CI/CD Integration
- **CI/CD Pipelines:** Integrate Docker builds into your CI/CD pipelines to automate testing, building, and deployment of images.
- **Automated Tests:** Write and run automated tests to validate your Docker images before deploying them.

## 15. Documentation and Comments
- **Document Dockerfiles:** Comment and document your Dockerfiles to explain the purpose of each instruction, making it easier for others to understand and maintain.

By following these best practices, you can ensure that your Docker containers are secure, efficient, and maintainable.