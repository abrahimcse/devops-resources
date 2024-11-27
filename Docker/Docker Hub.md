# Docker Hub

## What is Docker Hub?
Docker Hub is a cloud-based repository service for sharing, storing, and managing Docker images. It is the most widely used container registry and allows users to pull prebuilt images or push their custom images for collaboration.

## Key Features
**1. Public and Private Repositories :** Store Docker images for public access or private use.

**2. Automated Builds :** Automatically build images from GitHub/Bitbucket repositories.

**3. Image Sharing :** Share images across teams or publicly with the Docker community.

**4. Versioning :** Manage multiple versions of images using tags.

**5. Official Images :** Provides verified, secure, and maintained images from trusted publishers.

## Docker Hub Commands
### 1. Log in to Docker Hub:
```
docker login
```
- Your one-time device confirmation code is: `CODE`
- Press `ENTER` to open your browser or submit your device code here: `https://login.docker.com/activate`

Login Succeeded

### 2. Pull an Image from Docker Hub:
- `docker pull <image-name>:<tag>`
    ```
    docker pull nginx:latest
    ```

### 3. Push an Image to Docker Hub:
Tag the image first (if not already tagged):
- `docker tag <local-image>:<tag> <username>/<repo>:<tag>`
    ```
    docker tag my-app:latest abrahimcse/my-app:v1.0
    ```
### 4. Then push the image:
- `docker push <username>/<repo>:<tag>`
    ```
    docker push abrahimcse/my-app:v1.0
    ```
### 5. Search for Images on Docker Hub:
- `docker search <image-name>`
    ```
    docker search nginx
    ```
### 6. Log Out of Docker Hub:
```
docker logout
```

Docker Hub is essential for managing and distributing containerized applications efficiently.

