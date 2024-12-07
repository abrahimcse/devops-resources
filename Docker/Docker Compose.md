# Docker Compose
Docker Compose is a tool that allows you to define and manage multi-container applications using a YAML configuration file. It simplifies the orchestration of services, making it easier to build, deploy, and run applications with multiple containers.

# How to Install Docker Compose
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