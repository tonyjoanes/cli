# Docker Commands Guide

A comprehensive guide to working with Docker for building, running, and managing containerized applications.

## Table of Contents
- [Getting Started](#getting-started)
- [Working with Images](#working-with-images)
- [Working with Containers](#working-with-containers)
- [Docker Compose](#docker-compose)
- [Networking](#networking)
- [Volumes and Data Management](#volumes-and-data-management)
- [Docker Registry and Hub](#docker-registry-and-hub)
- [Building Images](#building-images)
- [Debugging and Logs](#debugging-and-logs)
- [System Management](#system-management)
- [Common Workflows](#common-workflows)
- [Docker Best Practices](#docker-best-practices)
- [Tips and Tricks](#tips-and-tricks)

---

## Getting Started

### Check Installation and Version
```bash
# Check Docker version
docker --version
docker version

# Display system-wide information
docker info

# Get help
docker --help
docker <command> --help
docker run --help
```

### Basic Concepts
- **Image**: A read-only template used to create containers
- **Container**: A runnable instance of an image
- **Dockerfile**: A text file containing instructions to build an image
- **Registry**: A service for storing and distributing Docker images
- **Volume**: Persistent data storage for containers

---

## Working with Images

### Listing and Searching Images

```bash
# List local images
docker images
docker image ls

# List all images including intermediates
docker images -a

# List image IDs only
docker images -q

# Search for images on Docker Hub
docker search nginx
docker search --filter stars=100 nginx
docker search --limit 5 postgres
```

### Pulling Images

```bash
# Pull an image from Docker Hub
docker pull nginx
docker pull ubuntu:22.04
docker pull node:18-alpine

# Pull from a specific registry
docker pull mcr.microsoft.com/dotnet/aspnet:8.0

# Pull all tags of an image
docker pull --all-tags nginx

# Pull a specific platform image
docker pull --platform linux/amd64 nginx
```

### Removing Images

```bash
# Remove an image
docker rmi nginx
docker image rm nginx

# Remove image by ID
docker rmi abc123

# Force remove an image
docker rmi -f nginx

# Remove multiple images
docker rmi nginx:latest ubuntu:22.04

# Remove all unused images
docker image prune

# Remove all images
docker rmi $(docker images -q)

# Remove dangling images (untagged)
docker image prune -a
```

### Inspecting Images

```bash
# View image details
docker inspect nginx

# View image history
docker history nginx

# View image layers
docker image history nginx --no-trunc

# Show image digest
docker images --digests
```

### Tagging Images

```bash
# Tag an image
docker tag nginx:latest myregistry/nginx:v1.0
docker tag abc123 myapp:production

# Tag for multiple registries
docker tag myapp:latest myregistry.com/myapp:latest
docker tag myapp:latest localhost:5000/myapp:latest
```

---

## Working with Containers

### Running Containers

```bash
# Run a container
docker run nginx

# Run in detached mode (background)
docker run -d nginx

# Run with a name
docker run --name my-nginx nginx

# Run and remove after exit
docker run --rm nginx

# Run interactively with terminal
docker run -it ubuntu bash
docker run -it node:18 node

# Run with port mapping
docker run -p 8080:80 nginx
docker run -p 127.0.0.1:8080:80 nginx

# Run with environment variables
docker run -e NODE_ENV=production node:18
docker run -e MYSQL_ROOT_PASSWORD=secret mysql

# Run with environment file
docker run --env-file .env myapp

# Run with volume mount
docker run -v /host/path:/container/path nginx
docker run -v myvolume:/data postgres

# Run with current directory mounted
docker run -v $(pwd):/app node:18

# Run with multiple options
docker run -d \
  --name my-app \
  -p 8080:80 \
  -v $(pwd):/app \
  -e NODE_ENV=production \
  node:18
```

### Listing Containers

```bash
# List running containers
docker ps

# List all containers (including stopped)
docker ps -a

# List container IDs only
docker ps -q
docker ps -aq

# List with custom format
docker ps --format "table {{.ID}}\t{{.Names}}\t{{.Status}}"

# List latest created container
docker ps -l

# List with size information
docker ps -s
```

### Managing Containers

```bash
# Start a stopped container
docker start my-container
docker start abc123

# Stop a running container
docker stop my-container

# Stop with timeout
docker stop -t 30 my-container

# Restart a container
docker restart my-container

# Pause a container
docker pause my-container

# Unpause a container
docker unpause my-container

# Kill a container (force stop)
docker kill my-container

# Remove a container
docker rm my-container

# Force remove a running container
docker rm -f my-container

# Remove all stopped containers
docker container prune

# Remove multiple containers
docker rm container1 container2 container3

# Stop and remove all containers
docker stop $(docker ps -q)
docker rm $(docker ps -aq)
```

### Executing Commands in Containers

```bash
# Execute a command in a running container
docker exec my-container ls -la

# Execute interactively
docker exec -it my-container bash
docker exec -it my-container sh

# Execute as a specific user
docker exec -u root my-container whoami

# Execute with environment variables
docker exec -e DEBUG=true my-container npm start

# Execute in a specific directory
docker exec -w /app my-container pwd
```

### Copying Files

```bash
# Copy from container to host
docker cp my-container:/app/file.txt ./file.txt
docker cp my-container:/app ./local-app

# Copy from host to container
docker cp ./file.txt my-container:/app/file.txt
docker cp ./config my-container:/etc/config
```

### Viewing Container Details

```bash
# View container logs
docker logs my-container

# Follow logs (like tail -f)
docker logs -f my-container

# View last N lines
docker logs --tail 100 my-container

# View logs with timestamps
docker logs -t my-container

# View logs since specific time
docker logs --since 1h my-container
docker logs --since 2024-01-01 my-container

# View container details
docker inspect my-container

# View resource usage statistics
docker stats my-container

# View all containers stats
docker stats

# View processes in container
docker top my-container

# View port mappings
docker port my-container
```

### Container Resource Limits

```bash
# Limit memory
docker run -m 512m nginx
docker run --memory="1g" nginx

# Limit CPU
docker run --cpus="1.5" nginx
docker run --cpu-shares=512 nginx

# Limit both memory and CPU
docker run -m 1g --cpus="2" nginx

# Update resource limits
docker update --memory="2g" my-container
docker update --cpus="1" my-container
```

---

## Docker Compose

### Basic Commands

```bash
# Start services defined in docker-compose.yml
docker compose up

# Start in detached mode
docker compose up -d

# Start and rebuild images
docker compose up --build

# Start specific services
docker compose up web db

# Stop services
docker compose down

# Stop and remove volumes
docker compose down -v

# Stop and remove images
docker compose down --rmi all

# View running services
docker compose ps

# View all services
docker compose ps -a

# View logs
docker compose logs

# Follow logs
docker compose logs -f

# View logs for specific service
docker compose logs -f web

# Execute command in service
docker compose exec web bash
docker compose exec db psql -U postgres

# Run one-off command
docker compose run web npm test
docker compose run --rm db psql -U postgres

# Restart services
docker compose restart

# Restart specific service
docker compose restart web

# Pause services
docker compose pause

# Unpause services
docker compose unpause

# Stop services without removing
docker compose stop
```

### Docker Compose File Management

```bash
# Use specific compose file
docker compose -f docker-compose.prod.yml up

# Use multiple compose files
docker compose -f docker-compose.yml -f docker-compose.prod.yml up

# Validate compose file
docker compose config

# View resolved compose file
docker compose config --services
docker compose config --volumes

# Build services
docker compose build

# Build without cache
docker compose build --no-cache

# Build specific service
docker compose build web

# Pull service images
docker compose pull

# Push service images
docker compose push
```

### Scaling Services

```bash
# Scale a service
docker compose up -d --scale web=3

# Scale multiple services
docker compose up -d --scale web=3 --scale worker=5
```

### Example docker-compose.yml

```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "8080:80"
    environment:
      - NODE_ENV=production
    volumes:
      - ./app:/app
    depends_on:
      - db
      - redis
    networks:
      - app-network

  db:
    image: postgres:15
    environment:
      - POSTGRES_PASSWORD=secret
      - POSTGRES_DB=myapp
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - app-network

  redis:
    image: redis:7-alpine
    networks:
      - app-network

volumes:
  db-data:

networks:
  app-network:
    driver: bridge
```

---

## Networking

### Network Management

```bash
# List networks
docker network ls

# Create a network
docker network create my-network

# Create network with driver
docker network create --driver bridge my-bridge
docker network create --driver overlay my-overlay

# Create network with subnet
docker network create --subnet=172.18.0.0/16 my-network

# Inspect network
docker network inspect my-network

# Connect container to network
docker network connect my-network my-container

# Disconnect container from network
docker network disconnect my-network my-container

# Remove network
docker network rm my-network

# Remove all unused networks
docker network prune
```

### Running Containers with Networks

```bash
# Run container on specific network
docker run -d --network my-network nginx

# Run with network alias
docker run -d --network my-network --network-alias web nginx

# Run with custom DNS
docker run -d --dns 8.8.8.8 nginx

# Run with hostname
docker run -d --hostname myhost nginx

# Connect to host network
docker run -d --network host nginx
```

---

## Volumes and Data Management

### Volume Management

```bash
# List volumes
docker volume ls

# Create a volume
docker volume create my-volume

# Inspect volume
docker volume inspect my-volume

# Remove volume
docker volume rm my-volume

# Remove all unused volumes
docker volume prune

# Remove all volumes
docker volume rm $(docker volume ls -q)
```

### Using Volumes with Containers

```bash
# Mount named volume
docker run -v my-volume:/data nginx

# Mount bind mount
docker run -v /host/path:/container/path nginx

# Mount as read-only
docker run -v my-volume:/data:ro nginx

# Create and mount volume
docker run -v my-data:/app/data nginx

# Mount current directory
docker run -v $(pwd):/app node:18

# Mount with specific options
docker run -v my-volume:/data:rw,Z nginx
```

### Backup and Restore

```bash
# Backup a volume
docker run --rm \
  -v my-volume:/data \
  -v $(pwd):/backup \
  ubuntu tar czf /backup/backup.tar.gz /data

# Restore a volume
docker run --rm \
  -v my-volume:/data \
  -v $(pwd):/backup \
  ubuntu tar xzf /backup/backup.tar.gz -C /

# Copy volume to another volume
docker run --rm \
  -v source-volume:/from \
  -v dest-volume:/to \
  alpine sh -c "cd /from && cp -av . /to"
```

---

## Docker Registry and Hub

### Docker Hub Operations

```bash
# Login to Docker Hub
docker login
docker login -u username -p password

# Login to private registry
docker login myregistry.com

# Logout
docker logout

# Push image to Docker Hub
docker push username/image:tag

# Pull image from Docker Hub
docker pull username/image:tag

# Tag for push
docker tag myapp:latest username/myapp:v1.0
docker push username/myapp:v1.0
```

### Private Registry

```bash
# Run local registry
docker run -d -p 5000:5000 --name registry registry:2

# Tag for local registry
docker tag myapp localhost:5000/myapp

# Push to local registry
docker push localhost:5000/myapp

# Pull from local registry
docker pull localhost:5000/myapp

# List images in registry
curl http://localhost:5000/v2/_catalog
```

---

## Building Images

### Dockerfile Basics

```dockerfile
# Example Dockerfile for Node.js app
FROM node:18-alpine

# Set working directory
WORKDIR /app

# Copy package files
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production

# Copy application files
COPY . .

# Expose port
EXPOSE 3000

# Set environment variables
ENV NODE_ENV=production

# Define user
USER node

# Command to run
CMD ["node", "server.js"]
```

### Building Images

```bash
# Build image from Dockerfile
docker build -t myapp .

# Build with tag
docker build -t myapp:v1.0 .

# Build from specific Dockerfile
docker build -f Dockerfile.prod -t myapp .

# Build with build arguments
docker build --build-arg VERSION=1.0 -t myapp .

# Build without cache
docker build --no-cache -t myapp .

# Build with target stage (multi-stage)
docker build --target production -t myapp .

# Build with platform
docker build --platform linux/amd64 -t myapp .

# Build with progress output
docker build --progress=plain -t myapp .

# Build and tag multiple
docker build -t myapp:latest -t myapp:v1.0 .
```

### Multi-stage Build Example

```dockerfile
# Build stage
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Production stage
FROM node:18-alpine AS production
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY --from=builder /app/dist ./dist
EXPOSE 3000
CMD ["node", "dist/server.js"]
```

### BuildKit Features

```bash
# Enable BuildKit
export DOCKER_BUILDKIT=1

# Build with BuildKit
docker build -t myapp .

# Use build secrets
docker build --secret id=npm,src=$HOME/.npmrc -t myapp .

# Use SSH for git
docker build --ssh default -t myapp .

# Build with cache from image
docker build --cache-from myapp:latest -t myapp .
```

---

## Debugging and Logs

### Viewing Logs

```bash
# View container logs
docker logs my-container

# Follow logs in real-time
docker logs -f my-container

# View last N lines
docker logs --tail 50 my-container

# View logs with timestamps
docker logs -t my-container

# View logs in time range
docker logs --since 10m my-container
docker logs --since "2024-01-01T10:00:00" my-container
docker logs --until 1h my-container

# Combine options
docker logs -f --tail 100 --since 1h my-container
```

### Debugging Containers

```bash
# Get a shell in running container
docker exec -it my-container sh
docker exec -it my-container bash

# Get a shell as root
docker exec -it -u root my-container bash

# Run debug commands
docker exec my-container ps aux
docker exec my-container netstat -tulpn
docker exec my-container df -h
docker exec my-container env

# Check container processes
docker top my-container

# View resource usage
docker stats my-container

# Inspect container
docker inspect my-container

# View container changes
docker diff my-container

# Export container filesystem
docker export my-container > container.tar

# Commit container to image (for debugging)
docker commit my-container debug-image
```

### Health Checks

```bash
# Run with health check
docker run -d \
  --name web \
  --health-cmd="curl -f http://localhost/ || exit 1" \
  --health-interval=30s \
  --health-timeout=3s \
  --health-retries=3 \
  nginx

# Check health status
docker ps
docker inspect --format='{{.State.Health.Status}}' web
```

### Debugging Build Issues

```bash
# Build with verbose output
docker build --progress=plain -t myapp .

# Build and stop at specific stage
docker build --target builder -t myapp-debug .

# Run intermediate stage
docker run -it myapp-debug sh

# Check build history
docker history myapp
```

---

## System Management

### System Information

```bash
# Show Docker disk usage
docker system df

# Show detailed disk usage
docker system df -v

# Display system-wide information
docker info

# Show Docker version
docker version
```

### Cleaning Up

```bash
# Remove all stopped containers
docker container prune

# Remove all unused images
docker image prune

# Remove all unused networks
docker network prune

# Remove all unused volumes
docker volume prune

# Remove all unused resources
docker system prune

# Remove everything including volumes
docker system prune -a --volumes

# Remove with force (no prompt)
docker system prune -f

# Remove unused build cache
docker builder prune
```

### Resource Management

```bash
# View resource usage
docker stats

# View specific container stats
docker stats my-container

# View stats without streaming
docker stats --no-stream

# View stats with formatting
docker stats --format "table {{.Name}}\t{{.CPUPerc}}\t{{.MemUsage}}"

# List containers with size
docker ps -s
```

### Events and Monitoring

```bash
# Stream Docker events
docker events

# Filter events by type
docker events --filter type=container

# Filter events by container
docker events --filter container=my-container

# Filter events by time
docker events --since 1h
docker events --since "2024-01-01T10:00:00"

# Format events
docker events --format '{{json .}}'
```

---

## Common Workflows

### Running a Web Application

```bash
# Pull and run nginx
docker run -d \
  --name my-web \
  -p 8080:80 \
  -v $(pwd)/html:/usr/share/nginx/html:ro \
  nginx:alpine

# View logs
docker logs -f my-web

# Test
curl http://localhost:8080
```

### Database Container Setup

```bash
# Run PostgreSQL
docker run -d \
  --name postgres \
  -e POSTGRES_PASSWORD=secret \
  -e POSTGRES_DB=myapp \
  -v pgdata:/var/lib/postgresql/data \
  -p 5432:5432 \
  postgres:15

# Run MySQL
docker run -d \
  --name mysql \
  -e MYSQL_ROOT_PASSWORD=secret \
  -e MYSQL_DATABASE=myapp \
  -v mysqldata:/var/lib/mysql \
  -p 3306:3306 \
  mysql:8

# Run MongoDB
docker run -d \
  --name mongodb \
  -e MONGO_INITDB_ROOT_USERNAME=admin \
  -e MONGO_INITDB_ROOT_PASSWORD=secret \
  -v mongodata:/data/db \
  -p 27017:27017 \
  mongo:7

# Connect to database
docker exec -it postgres psql -U postgres -d myapp
docker exec -it mysql mysql -u root -p
docker exec -it mongodb mongosh -u admin -p secret
```

### Development Environment with Docker Compose

```yaml
# docker-compose.yml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "3000:3000"
    volumes:
      - .:/app
      - /app/node_modules
    environment:
      - DATABASE_URL=postgres://user:pass@db:5432/myapp
    depends_on:
      - db
    command: npm run dev

  db:
    image: postgres:15
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=pass
      - POSTGRES_DB=myapp
    volumes:
      - pgdata:/var/lib/postgresql/data

volumes:
  pgdata:
```

```bash
# Start development environment
docker compose up -d

# View logs
docker compose logs -f app

# Run migrations
docker compose exec app npm run migrate

# Stop environment
docker compose down
```

### CI/CD Pipeline Example

```bash
# Build for CI/CD
docker build -t myapp:${CI_COMMIT_SHA} .

# Tag for registry
docker tag myapp:${CI_COMMIT_SHA} registry.com/myapp:latest

# Login to registry
echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin

# Push to registry
docker push registry.com/myapp:${CI_COMMIT_SHA}
docker push registry.com/myapp:latest

# Run tests in container
docker run --rm myapp:${CI_COMMIT_SHA} npm test
```

### Container Backup and Migration

```bash
# Export container
docker export my-container > container-backup.tar

# Import container
docker import container-backup.tar my-image:backup

# Save image to tar
docker save -o myapp.tar myapp:latest

# Load image from tar
docker load -i myapp.tar

# Save multiple images
docker save -o images.tar myapp:latest nginx:alpine

# Transfer image to another host
docker save myapp:latest | gzip | ssh user@host docker load
```

---

## Docker Best Practices

### Image Best Practices

1. **Use Official Base Images**
   ```dockerfile
   FROM node:18-alpine  # Prefer alpine variants
   FROM python:3.11-slim  # Or slim variants
   ```

2. **Multi-stage Builds**
   ```dockerfile
   FROM node:18 AS builder
   # Build steps...

   FROM node:18-alpine
   COPY --from=builder /app/dist ./dist
   ```

3. **Minimize Layers**
   ```dockerfile
   # Bad - multiple layers
   RUN apt-get update
   RUN apt-get install -y package1
   RUN apt-get install -y package2

   # Good - single layer
   RUN apt-get update && apt-get install -y \
       package1 \
       package2 \
       && rm -rf /var/lib/apt/lists/*
   ```

4. **Use .dockerignore**
   ```
   node_modules
   npm-debug.log
   .git
   .env
   *.md
   ```

5. **Don't Run as Root**
   ```dockerfile
   RUN addgroup -g 1001 -S nodejs
   RUN adduser -S nodejs -u 1001
   USER nodejs
   ```

6. **Use Specific Tags**
   ```dockerfile
   # Bad
   FROM node:latest

   # Good
   FROM node:18.17.1-alpine3.18
   ```

### Security Best Practices

1. **Scan Images for Vulnerabilities**
   ```bash
   docker scan myapp:latest
   ```

2. **Use Secrets Securely**
   ```bash
   # Don't put secrets in Dockerfile
   # Use build secrets or environment variables
   docker build --secret id=token,src=token.txt .
   ```

3. **Limit Container Capabilities**
   ```bash
   docker run --cap-drop=ALL --cap-add=NET_BIND_SERVICE myapp
   ```

4. **Use Read-only Filesystem**
   ```bash
   docker run --read-only --tmpfs /tmp myapp
   ```

### Performance Best Practices

1. **Leverage Build Cache**
   ```dockerfile
   # Copy dependencies first
   COPY package*.json ./
   RUN npm ci

   # Then copy source code
   COPY . .
   ```

2. **Use BuildKit**
   ```bash
   export DOCKER_BUILDKIT=1
   docker build -t myapp .
   ```

3. **Optimize Image Size**
   ```bash
   # Use alpine base images
   # Remove unnecessary files
   # Combine RUN commands
   # Use multi-stage builds
   ```

---

## Tips and Tricks

### Useful Aliases

```bash
# Add to ~/.bashrc or ~/.zshrc

# Docker shortcuts
alias d='docker'
alias dc='docker compose'
alias dps='docker ps'
alias dpa='docker ps -a'
alias di='docker images'
alias dex='docker exec -it'
alias dl='docker logs -f'

# Cleanup
alias docker-clean='docker system prune -af --volumes'
alias docker-stop-all='docker stop $(docker ps -q)'
alias docker-rm-all='docker rm $(docker ps -aq)'
```

### Quick Commands

```bash
# Remove all stopped containers
docker rm $(docker ps -aq -f status=exited)

# Stop all running containers
docker stop $(docker ps -q)

# Remove all dangling images
docker rmi $(docker images -f dangling=true -q)

# Remove all unused volumes
docker volume rm $(docker volume ls -q -f dangling=true)

# Show container IPs
docker inspect -f '{{.Name}} - {{.NetworkSettings.IPAddress}}' $(docker ps -q)

# Follow logs from multiple containers
docker compose logs -f service1 service2

# Execute command in all running containers
docker ps -q | xargs -I {} docker exec {} command

# Copy files from all containers
docker ps -q | xargs -I {} docker cp {}:/path/to/file ./{}

# Show port mappings for all containers
docker ps --format 'table {{.Names}}\t{{.Ports}}'
```

### Environment-specific Compose

```bash
# Development
docker compose -f docker-compose.yml -f docker-compose.dev.yml up

# Production
docker compose -f docker-compose.yml -f docker-compose.prod.yml up

# Testing
docker compose -f docker-compose.yml -f docker-compose.test.yml up
```

### Debugging Tips

```bash
# Enter container and explore
docker run -it --entrypoint sh myapp

# Override entrypoint
docker run -it --entrypoint /bin/bash myapp

# Mount current directory for debugging
docker run -it -v $(pwd):/debug myapp

# Check why container exited
docker logs --tail 50 container-name

# Compare container changes
docker diff container-name

# Check container resource limits
docker inspect --format='{{.HostConfig.Memory}}' container-name
```

### One-liner Tools

```bash
# Run temporary container with auto-remove
docker run --rm -it ubuntu bash

# Quick file server
docker run --rm -p 8080:8080 -v $(pwd):/web python:3 \
  python -m http.server 8080 --directory /web

# Quick PostgreSQL client
docker run -it --rm postgres:15 psql -h host -U user -d database

# Quick MySQL client
docker run -it --rm mysql:8 mysql -h host -u user -p

# Quick Redis client
docker run -it --rm redis:7 redis-cli -h host

# Run curl from container
docker run --rm appropriate/curl -s http://example.com

# Test network connectivity
docker run --rm busybox ping -c 3 google.com

# Generate password
docker run --rm alpine/openssl rand -base64 32
```

---

## Quick Reference

### Essential Commands
```bash
docker pull <image>                # Pull image
docker run <image>                 # Run container
docker ps                          # List running containers
docker ps -a                       # List all containers
docker images                      # List images
docker logs <container>            # View logs
docker exec -it <container> bash   # Get shell
docker stop <container>            # Stop container
docker rm <container>              # Remove container
docker rmi <image>                 # Remove image
```

### Common Patterns
```bash
# Run web server
docker run -d -p 8080:80 nginx

# Run with volume
docker run -v $(pwd):/app myapp

# Run with environment variable
docker run -e ENV_VAR=value myapp

# Run and remove after exit
docker run --rm -it ubuntu bash

# Build and run
docker build -t myapp . && docker run -d myapp
```

### Cleanup Commands
```bash
docker system prune               # Remove unused data
docker system prune -a            # Remove all unused images
docker system prune --volumes     # Remove unused volumes
docker container prune            # Remove stopped containers
docker image prune                # Remove unused images
docker volume prune               # Remove unused volumes
docker network prune              # Remove unused networks
```

---

## Additional Resources

- [Official Docker Documentation](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/)
- [Dockerfile Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [Docker Security](https://docs.docker.com/engine/security/)
