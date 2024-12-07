# Docker Compose
Docker Compose is a tool that allows you to define and manage multi-container applications using a YAML configuration file. It simplifies the orchestration of services, making it easier to build, deploy, and run applications with multiple containers.

# 1. How to Install Docker Compose
## Step 1: Install Docker (Pre-requisite)
Ensure Docker is installed and running on your system:

- Check Docker version:
```bash
docker --version
```
- Install Docker if not already installed:
    - For Ubuntu/Debian:
    ```bash
    sudo apt update
    sudo apt install docker.io
    ```
## Step 2: Install Docker Compose
### Option 1: Using Package Manager (Linux)
For Ubuntu or Debian:
```bash
sudo apt update
sudo apt install docker-compose
```
### For CentOS/RHEL:
```bash
sudo yum install docker-compose
```

### Option 2: Manual Installation (Linux/MacOS/Windows via WSL)
1. Download the latest version of Docker Compose:
    ```bash
    sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    ```
2. Set executable permissions:
    ```bash
    sudo chmod +x /usr/local/bin/docker-compose
    ```
3. Verify the installation:
    ```bash
    docker-compose --version
    ```
## 2. Basic docker-compose.yml Structure

```yaml
version: "3.9"  # Specify the Docker Compose version

services:
  app:  # Define the service name
    image: nginx  # Use the nginx image
    ports:
      - "8080:80"  # Map host port 8080 to container port 80
    volumes:
      - ./html:/usr/share/nginx/html  # Bind volume for serving custom content
    networks:
      - app-network

networks:
  app-network:
    driver: bridge  # Use the bridge driver
```
## 3. Docker Compose Commands

### 1. Start Services:
```bash
docker-compose up
```
**Use `-d` for detached mode:**
```bash
docker-compose up -d
```
### 2. Stop Services:
```bash
docker-compose down
```
### 3. View Logs:
```bash
docker-compose logs
```
**For specific services:**
```bash
docker-compose logs <service-name>
```
### 4. Scale Services:
```bash
docker-compose up --scale <service-name>=<number>
```
**Example:**
```bash
docker-compose up --scale app=3
```
### 5. Restart Services:
```bash
docker-compose restart
```
### 6. Build Images:
If using a Dockerfile in your service:
```bash
docker-compose build
```

## 4. Example: Multi-Service Application
`docker-compose.yml`
```yaml
version: "3.9"

services:
  web:
    image: nginx
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
    networks:
      - app-network

  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: app_db
      MYSQL_USER: user
      MYSQL_PASSWORD: password
    networks:
      - app-network

networks:
  app-network:
    driver: bridge
```
- **Command**
```bash
docker-compose up -d        # Start the application:
docker-compose ps           # Verify running containers:
docker-compose down         # Stop the application:
```
Docker Compose is an essential tool for deploying multi-container applications, enabling developers to manage services and resources effectively with minimal configuration.