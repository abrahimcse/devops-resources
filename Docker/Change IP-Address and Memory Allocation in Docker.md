# How to Change the Default IP Address in Docker

## Option - 1: Modify Docker Daemon Configuration
### 1. Edit the Docker Daemon File
Open the configuration file:
```bash
sudo vim /etc/docker/daemon.json
```
### 2. Set a Custom Bridge IP
Add the following configuration:

```json
Copy code
{
  "bip": "192.168.1.1/24"
}
```
### 3. Restart Docker
Save the changes and restart the Docker service:
```bash
sudo systemctl restart docker
```
## Option - 2: Create a Custom Bridge Network
- To control the IP range, define a custom network in the `docker-compose.yml`:
```yaml
networks:
  custom-network:
    driver: bridge
    ipam:
      config:
        - subnet: 192.168.1.0/24
```
Assign services to the custom network:
```yaml
services:
  web:
    image: nginx
    networks:
      - custom-network
networks:
  custom-network:
    driver: bridge
```

- `docker run` : use the **default bridge network** (bridge mode).
- `docker-compose up` : use **custom bridge network** for the services defined in the `docker-compose.yml`. file.

## Memory Allocation in Docker Compose
### 1. docker - compose file
```yml
version: "3.9"  # Specify the Docker Compose version

services:
  app:
    image: myapp:latest  # Replace with your image
    deploy:
      resources:
        limits:
          memory: 512M  # Maximum memory limit (e.g., 512 MB)
        reservations:
          memory: 256M  # Minimum memory reserved (e.g., 256 MB)
    ports:
      - "8080:80"
```

### 2. Verifying Memory Limits

**Inspect the Container:**
```bash
docker inspect <container-name> | grep Memory
```
**Monitor Container Resource Usage:**
```bash
docker stats
```