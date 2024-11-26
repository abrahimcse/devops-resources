# Step-by-Step Guide to Managing Docker Containers with Commands

## What is a Container?
`A container` is a lightweight, portable, and isolated environment that allows you to run applications consistently across different systems. 
It packages an application and its dependencies (such as libraries, binaries, and configuration files) together, ensuring that it runs the same way regardless of the underlying infrastructure.
- A runtime instance of a Docker image.
## Docker Containers with Commands
### 1. Pulling an Image
Before running a container, you need an image:
```
docker pull nginx
```
This command pulls the official `nginx` image from Docker Hub.
- View NGINX Configuration File
    ```
    cat /etc/nginx/conf.d/default.conf
    ```

### 2. Running a Container
- Basic run:
    ```
    docker run nginx
    ```
This runs the container interactively using the nginx image.

- Run in detached mode:
    ```
    docker run -d nginx
    ```
Runs the container in the background.

- Run with port mapping:
    ```
    docker run -d -p 8080:80 nginx
    ```
Maps port `8080` on the host to port `80` in the container.

- Run with a custom name:
    ```
    docker run -d --name mynginx -p 8080:80 nginx
    ```
- The Container to restart automatically
    ```
    docker run -d --name mynginx --restart=always -p 8090:80 nginx
    ```
### Explanation of the Command
1. `docker run`:
    This command is used to create and start a new container from a specified image.
2. `-d`: Runs the container in detached mode (in the background).

3. `--name mynginx`: Assigns a custom name (`mynginx`) to the container for easy identification.
4. `--restart=always`: Configures the container to restart automatically if it stops, including after a system reboot.
5. `-p 8090:80`: Maps port 8090 on the host machine to port `80` inside the container.
    - Host Port (`8090`): Accessible on your local system or network.
    - Container Port (`80`): The default port used by the `nginx` server.
6. `nginx`:
The name of the Docker image to use for the container (in this case, the official NGINX image).
### Result
- The NGINX web server will be running in the background.
- You can access it via `http://<your-host-ip>:8090` or `http://<server-ip>:8090` in a browser or using tools like `curl`.


### 3. Viewing Running Containers
```
docker ps
docker ps -a
```
- `-a` To see all containers (including stopped ones)
### 4. Stopping , Restarting and Removing Containers
```
docker stop <container-id or name>
docker start <container-id or name>
docker rm -f <container-id or name>
```
- `rm` removing `-f` forcefully 

### 5. Accessing a Container
```
docker exec -it <container-name or container-id> bash
```
- Check `OS` details within the container (Inside the container)
    ```
    cat /etc/os-release
    ```
### 6. Copying Files
- Copy a file from the container to the host (`CONTAINER -> HOST`) :
`docker cp <container-name>:/path/to/file /host/path`
    ```
    docker cp mynginx:/ym.txt . 
    ```
    - `.` present directory
- Copy a file from the host to the container(`HOST -> CONTAINER`):
    ```
    docker cp /host/path <container-name>:/path/to/destination
    ```
- Copy the Edited HTML File to the NGINX Container:
    ```
    docker cp ym.html mynginx:/usr/share/nginx/html
    ```
**Access the HTML File:** Open a browser and go to `http://localhost:8090/ym.html` (or your configured port) to verify the HTML page is being served correctly.
### 7. Committing Changes to a New Image
- Create a new image from a modified container :
    `docker commit <container-name> <new-image-name>`
    ```
    docker commit mynginx ym-nginx
    ```
- List images:
    ```
    docker images
    ```
### 8. Saving and Loading Images
- Save an image to a tarball:
`docker save <image-name> | gzip > image.tar.gz`
    ```
    docker save nginx | gzip > nginx.tar.gz
    ```
- Load an image from a tarball:
    ```
    docker load -i  nginx.tar.gz
    ```
### 10. Removing Containers and Images
- Remove a stopped container:
    ```
    docker rm <container-id or name>
    ```
- Force-remove a container:
    ```
    docker rm -f <container-id or name>
    ```
- Remove an image:
    ```
    docker rmi <image-id or name>
    ```
### 11. Inspecting and Monitoring Containers
- Inspect detailed information about a container:
    ```
    docker inspect <container-id or name>
    ```
- View logs of a container:
    ```
    docker logs <container-id or name>
    ```
- Follow logs in real-time:
    ```
    docker logs -f <container-id or name>
    ```
- Monitor resource usage of running containers:
    ```
    docker stats
    ```
### 12. Automatic Restarts
- Run a container with auto-restart enabled:
    ```
    docker run -d --name mynginx --restart=always -p 8080:80 nginx
    ```
### 13. Cleaning Up Docker Resources
- Remove all stopped containers:
    ```
    docker container prune
    ```
- Remove all unused images:
    ```
    docker image prune
    ```
- Remove unused resources (containers, images, volumes, and networks):
    ```
    docker system prune
    ```
These steps cover the entire lifecycle of Docker container management, from basic usage to advanced workflows.