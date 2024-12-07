# Docker Volumes and Bind Mounts

Docker provides methods to manage persistent data for containers, enabling data sharing and persistence even after containers are deleted. Two commonly used options are `Volumes` and `Bind Mounts`. Here's a detailed breakdown:



## 1. What is Docker Volume?
- A volume is a storage mechanism managed by Docker that allows containers to store data outside their writable layer.
- Located in Docker's directory (e.g., `/var/lib/docker/volumes/`).
### i. Commands for Docker Volumes

**a. Create a Volume** : `docker volume create <volume-name>`
```bash
docker volume create my-volume
```
**b. List All Volumes** 
```bash
docker volume ls
```
**c. Inspect a Volume** `docker volume inspect <volume-name>`
```bash
docker volume inspect my-volume
```
**d. Remove a Volume**
`docker volume rm <volume-name>`
```bash
docker volume rm my-volume
```
**e. Remove Unused Volumes**
```bash
docker volume prune
```
### ii. Using Volumes in Containers
Attach a volume to a container:
```bash
docker run -d --name <container-name> -v <volume-name>:<container-path> <image-name>
```
**Example:**

```bash
docker run -d --name web-app -v my-volume:/usr/share/nginx/html nginx
```
- `my-volume:` Volume name created by Docker.
- `/usr/share/nginx/html`: Directory inside the container where data is mounted.

## 5. What is Bind Mount?
- A `bind mount` allows you to mount any directory from the `host` system to a `container`.
- Unlike volumes, bind mounts rely on the `host's` file structure.
### i. Using Bind Mounts in Containers

Attach a bind mount to a container:
```bash
docker run -d --name <container-name> -v <host-path>:<container-path> <image-name>
```
**Example:**
```bash
docker run -d --name web-app -v /home/user/data:/usr/share/nginx/html nginx
```
- `/home/user/data`: Host directory to be mounted.
- `/usr/share/nginx/html`: Directory inside the container.