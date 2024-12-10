# Running Docker Without Root Privileges
## Option 1: Configure Docker Rootless Mode

### Step 1: Install Dependencies
Install the required tools for rootless Docker:

```bash
sudo apt-get update
sudo apt-get install -y uidmap
```
### Step 2: Install Docker Rootless Mode
Run the rootless setup tool:
```bash
dockerd-rootless-setuptool.sh install
```

## Option 2: Add User to Docker Group
### Step 1: Create or Verify Docker Group
Check if the docker group exists:
```bash
cat /etc/group | grep docker
```
If not, create it:
```bash
sudo groupadd docker
```
### Step 2: Add Your User to the Docker Group
```bash
sudo usermod -aG docker $USER
```
### Step 3: Restart Docker Service
```bash
sudo systemctl restart docker
```

### Step 4: Verify Docker Access
```bash
docker run hello-world
```
You should see the output without using `sudo`.

## Summary
- `Rootless Mode`: Enhanced security, no root privileges required.
- `Docker Group`: Quick setup but slightly less secure.