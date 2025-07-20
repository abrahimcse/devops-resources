# What is Docker Network?

A `Docker Network` is a way for containers to talk to each other.
Imagine you have two containers:
- One has a database (like MySQL)
- One has a web app (like Node.js)

Your app needs to connect to the database.
But how will it `find` the database container?

👉 That’s where `Docker Network` helps.

It creates a `connection path` so containers can `communicate safely`.


## Why do we need Docker Network?
Because in real apps:
- You don’t run just one container.
- You may run 5 or 10 containers together (like app, database, backend, frontend, redis, etc).
- They need to talk to each other to work properly.

---

### bypes of Docker Network (Most Common)
- 1️⃣ bridge (default)
- 2️⃣ host
- 3️⃣ none

### 1️⃣ bridge (default)

- This is the default network Docker uses.
- It puts containers in a private network.
- Containers can talk to each other by name.

📌 Example:
```bash
docker network create mynetwork
docker run -d --name db --network mynetwork mysql
docker run -d --name app --network mynetwork node-app
```
Now `app` can talk to `db` using just the name `db`.


### 2️⃣ host

- The container uses the host machine's network directly.
- Faster, but less secure.
- Mostly used in Linux.

📌 Example:
```bash
docker run --rm --network host nginx
```
### 3️⃣ none
- No network at all.
- The container is fully isolated.
- Used for testing or security purposes.
📌 Example:
```bash
docker run --rm --network none ubuntu
```
### Useful Docker Network Commands

```markdown
| Command	                        | What It Does
| ------------------------------------------------------------------------|
| docker network ls	                | Show all networks                   |
| docker network create mynet	    | Create a new custom network         |
| docker network inspect mynet	    | Show network details                |
| docker run --network mynet ...	| Run a container in a network        |

```

### Real Example:
**Imagine:**
- You create a custom network called app_net
- You run a MySQL container and a Node.js container in the same network.

They can now `talk to each other easily`.
```bash
docker network create app_net
docker run -d --name mysql --network app_net mysql
docker run -d --name nodejs --network app_net my-node-app
```
Now inside `nodejs`, you can connect to MySQL using:
```ini
host = "mysql"
```

### Simple Summary:
Docker Network allows `containers to communicate` with each other and with the `outside world`, using different types of networks like `bridge`, `host`, or `none`.

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