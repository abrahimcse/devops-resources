# Docker Network
Docker networking allows containers to communicate with each other, the host machine, and external systems. It provides flexibility for containerized applications to operate seamlessly, whether in isolation or as part of a larger ecosystem.

## Commands for Docker Networking

### a. List Networks
```bash
docker network ls
```
### b. Inspect a Network
```bash
docker network inspect <network-name>
```
### c. Create a Network
```bash
docker network create <network-name>
```
**Example:**

```bash
docker network create my-custom-network
```
### d. Remove a Network
```bash
docker network rm <network-name>
```
### e. Connect a Container to a Network
```bash
docker network connect <network-name> <container-name>
```
**Example:**

```bash
docker network connect my-custom-network my-container
```
### f. Disconnect a Container from a Network
```bash
docker network disconnect <network-name> <container-name>
```

## Practical Example
### Scenario: Connect Two Containers on a Bridge Network
1. **Create a custom bridge network:**
    ```bash
    docker network create my-app-network
    ```
2. **Run two containers on the same network:**
    ```bash
    docker run -d --name app1 --network my-app-network nginx
    docker run -d --name app2 --network my-app-network alpine
    ```
3. **Test connectivity between containers:**
    ```bash
    docker exec -it app2 ping app1
    ```

Docker networking provides the tools to create flexible, secure, and robust networking setups for containerized applications.