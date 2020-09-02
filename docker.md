# Docker

## Images

```bash
# Pulls a new image
docker pull <image>:<tag>

# Build from local Dockerfile
docker build -t <image name>

# Force building for a specific environment
docker build -f Dockerfile.dev .

# Show list of docker images
docker images

# Show list of all docker images
docker images -a

# Removes all unused images
docker image prune -a

# Remove specific image
docker rmi <image id>

# Tags a given image
docker tag <image id> username/container_name:tag
```

## Containers

```bash
# Show list of running containers
docker ps

# Show list of all running containers
docker ps --all

# Push a container image to Docker Hub (login first)
docker push username/container_name:tag

# Show list all containers
docker container ls -a

# Run a container using image name
docker run <image name>

# Run a named container
docker run --name <container name>

# Start a container in the background
docker run -d <image name>

# Stop a container
docker stop <container id>

# Show log for container
docker logs <container id>
```

## Dockerfile

```bash
# Copy all files from local machine to image path
COPY ./ ./

# Specify the working directory
WORKDIR path/to/folder
```

## Misc

```bash
# Logs into Docker Hub
docker login

# Get system info
docker system info

# Show how much disk space is being used by docker (images, volumes etc.)
docker system df

# Remove all containers
docker system prune
```

