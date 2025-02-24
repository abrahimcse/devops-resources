# GitHub Actions deploying on VPS
Here's a **step-by-step** guide for setting up **CI/CD with GitHub Actions**, deploying on `VPS`, using `Docker`, `Docker Hub`, and` Nginx` for your `React (Yarn)` + `Spring Boot (JDK 17)` + `MongoDB` application. 

## ✅ Recommended Structure
```mathematica
/my-project
  ├── frontend  (React with Yarn)
  │   ├── Dockerfile  ✅ (For frontend)
  ├── backend   (Spring Boot JDK 17)
  │   ├── Dockerfile  ✅ (For backend)
  ├── docker-compose.yml  ✅ (For managing services)
  ├── nginx.conf  ✅ (For reverse proxy)
  ├── .github/workflows/cicd.yml  ✅ (For CI/CD)

```
## Step 1: Set Up Dockerfiles
### 1. Dockerfile for Frontend (React with Yarn)
- Create `Dockerfile` in the **frontend** folder:

```dockerfile
# Set the base image for Node.js
FROM node:20

# Set the working directory in the container
WORKDIR /app

# Copy package.json and yarn.lock first to install dependencies
COPY package.json yarn.lock ./

# Install dependencies using Yarn
RUN yarn install

# Copy the rest of the application code
COPY . .

# Build the React app
RUN yarn build

# Expose the port on which the React app will run
EXPOSE 3000

# Command to start the React app
CMD ["yarn", "start"]


```

### 2. Dockerfile for Backend (Spring Boot JDK 17)
- Create `Dockerfile` in the **backend** folder:

```dockerfile
# Use OpenJDK 17 as the base image for Spring Boot
FROM openjdk:17-jdk-slim

# Set the working directory
WORKDIR /app

# Copy the Spring Boot jar file (use your actual jar file name)
COPY target/my-backend-app.jar /app/backend-app.jar

# Expose the port that Spring Boot app will use (default is 8080)
EXPOSE 8080

# Command to run the Spring Boot app
CMD ["java", "-jar", "backend-app.jar"]

```

## Step 2: Setting Up `MongoDB` in `docker-compose.yml`
- Create `docker-compose.yml`
```yml
version: '3'
services:
  frontend:
    build:
      context: ./frontend
    ports:
      - "3000:3000"
    networks:
      - app-network

  backend:
    build:
      context: ./backend
    ports:
      - "8080:8080"
    environment:
      - SPRING_DATA_MONGODB_URI=mongodb://mongo:27017/mydb
    networks:
      - app-network
    depends_on:
      - mongo

  mongo:
    image: mongo:latest
    volumes:
      - mongo-data:/data/db
    networks:
      - app-network

  nginx:
    image: nginx:latest
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
    ports:
      - "80:80"
    networks:
      - app-network

networks:
  app-network:
    driver: bridge

volumes:
  mongo-data:
```

## Step 3: Setting Up NGINX Configuration
- Create `nginx.conf`
```nginx
server {
  listen 80;  # Listen on port 80 (standard HTTP port)

  # Handle requests for the frontend (React app)
  location / {
    proxy_pass http://frontend:3000;  # Forward requests to the frontend service on port 3000
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
  }

  # Handle requests for the backend (Spring Boot app)
  location /api/ {
    proxy_pass http://backend:8080;  # Forward requests to the backend service on port 8080
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
  }
}


```
## Step 4: Setting Up GitHub Actions for CI/CD
- Create `.github/workflows/cicd.yml`

```yaml
name: CI/CD Pipeline  # Name of the workflow

on:
  push:
    branches:
      - main  # Trigger this workflow when code is pushed to the 'main' branch
  pull_request:
    branches:
      - main  # Also trigger on pull requests to the 'main' branch

jobs:
  build:  # Define the 'build' job
    runs-on: ubuntu-latest  # This job will run on the latest version of Ubuntu

    steps:
    - name: Checkout code  # Step to checkout the code from the repository
      uses: actions/checkout@v2

    - name: Set up Docker Buildx  # Set up Docker buildx (a tool that enables building multi-platform Docker images)
      uses: docker/setup-buildx-action@v2

    - name: Cache Docker layers  # Cache Docker layers to speed up the build process
      uses: actions/cache@v2
      with:
        path: /tmp/.buildx-cache  # Path to cache Docker layers
        key: ${{ runner.os }}-buildx-${{ github.sha }}  # Cache key based on OS and commit hash
        restore-keys: |
          ${{ runner.os }}-buildx-  # Restore cache if available

    - name: Build frontend Docker image  # Build the frontend Docker image
      run: |
        docker build -t <dockerhub-username>/frontend ./frontend

    - name: Build backend Docker image  # Build the backend Docker image
      run: |
        docker build -t <dockerhub-username>/backend ./backend

    - name: Push frontend Docker image to Docker Hub  # Push the frontend Docker image to Docker Hub
      run: |
        docker push <dockerhub-username>/frontend

    - name: Push backend Docker image to Docker Hub  # Push the backend Docker image to Docker Hub
      run: |
        docker push <dockerhub-username>/backend

```

## Step 5: Deploying to VPS
`1. Install Docker on VPS`

SSH into your VPS and install Docker if it's not already installed. You can do this by running the following commands:
```bash
sudo apt update
sudo apt install docker.io
```
`2. Install Docker Compose on your VPS:`
```bash
sudo apt install docker-compose
```
## Step 6: Setting Up NGINX on VPS
### 1. Install NGINX on VPS
```bash
sudo apt update
sudo apt install nginx
```
### 2. Configure NGINX
Upload the `nginx.conf` file to the `/etc/nginx/` directory.

### 3. Restart NGINX
```bash
sudo systemctl restart nginx
```