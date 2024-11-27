# Docker Images

## What Are Docker Images?
A Docker image is a lightweight, stand-alone, and immutable package that contains everything needed to run a piece of software, including the application code, runtime, libraries, environment variables, and configurations. It serves as a template for creating Docker containers.

## Key Features of Docker Images:
### 1. Layered Structure:
- Images are built in layers using a `Dockerfile`.
- Each layer represents a change (e.g., installing software or adding files).
### 2. Read-Only:
- Docker images are immutable. When a container is created from an image, a writable layer is added on top.
### 3. Reusable:
- Images can be reused across `multiple` containers.
### 4. Portable:
- Images can be shared via Docker registries like `Docker Hub`, enabling consistent deployments across systems.

## Commands for Docker Images

### 1. List Images:
```
docker images
```
### 2. Pull an Image:
- `docker pull <image-name>`
    ```
    docker pull nginx
    docker run -d -p 8080:80 nginx
    ```
- Pulls the `NGINX` image from `Docker Hub`.
- Runs it in a container, mapping host port `8080` to container port `80`.
### 3. Build a Custom Image:

- Create a Dockerfile:
    ```
    FROM ubuntu:latest
    RUN apt-get update && apt-get install -y nginx
    CMD ["nginx", "-g", "daemon off;"]
    ```
- Build the image:
    ```
    docker build -t custom-nginx .
    ```
- Run a container from it:
    ```
    docker run -d -p 8090:80 custom-nginx
    ```
Builds a new image from a `Dockerfile` in the current directory.

### 4. Remove an Image:
- `docker rmi <image-id or image-name>`

    ```
    docker rmi nginx
    ```
### 5. Tag an Image:
- `docker tag <existing-image> <new-repo-name>:<tag>`

    ```
    docker tag my-app my-repo/my-app:v1.0
    ```
Assigns a new name or tag to an existing image.

### 6. Save an Image to a File:
- `docker save <image-name> | gzip > <file-name>.tar.gz`

    ```
    docker save my-app | gzip > my-app.tar.gz
    ```
Exports the image as a .tar.gz file.

### 7. Load an Image from a File:
- `docker load -i <file-name>.tar.gz`

    ```
    docker load -i my-app.tar.gz
    ```
Imports an image from a .tar.gz file.

### 8. Inspect an Image:
- `docker inspect <image-id or image-name>`

    ```
    docker inspect my-app
    ```

Displays detailed metadata about an image.