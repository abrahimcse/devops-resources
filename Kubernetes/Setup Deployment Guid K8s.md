# Kubernetes Setup and Deployment Guide

This guide provides step-by-step instructions to install and use Kubernetes, Minikube, and deploy a web application using Kubernetes.

## Prerequisites

1. [Basic knowledge of Linux commands](https://github.com/abrahimcse/Linux-CLI-Cheatsheet).
2. A Linux system with root privileges.
3. [Docker installed and running on your system](https://github.com/abrahimcse/devops-resources/blob/main/Docker/1.Docker%20Basic.md).

---

## Step 1: Install kubectl on Linux

### Add Kubernetes Repository

```bash
cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://pkgs.k8s.io/core:/stable:/v1.31/rpm/
enabled=1
gpgcheck=1
gpgkey=https://pkgs.k8s.io/core:/stable:/v1.31/rpm/repodata/repomd.xml.key
EOF
```

### Install kubectl

```bash
sudo yum install -y kubectl
```

### Verify Installation

```bash
kubectl client --version
kubectl cluster-info
```

---

## Step 2: Install Minikube

### Download and Install Minikube

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-latest.x86_64.rpm
sudo rpm -Uvh minikube-latest.x86_64.rpm
```

### Verify Minikube Installation

```bash
minikube status
minikube start
```

### Start Docker

```bash
sudo systemctl start docker
```

---

## Step 3: Kubernetes Deployment

### Create a Deployment

```bash
kubectl create deployment my-nginx --image=nginx:latest
```

### Manage Deployments

- Delete Deployment:
  ```bash
  kubectl delete deployment my-nginx
  ```
- List Deployments:
  ```bash
  kubectl get deployments
  ```
- List Pods:
  ```bash
  kubectl get pods
  ```

### Access Kubernetes Dashboard

```bash
kubectl dashboard
```

### Expose Deployment as a Service

```bash
kubectl expose deployment my-nginx --port=80 --type=LoadBalancer
kubectl get services
minikube service my-nginx
```

---

## Step 4: Deploy a Demo Web App

### Create a Node.js Web App

1. Install Node.js:
   ```bash
   node -v
   ```
2. Create a React app:
   ```bash
   npx create-react-app myapp
   cd myapp/
   npm start
   ```

### Create a Docker Image

1. Create a `Dockerfile` in your project directory:
   ```dockerfile
   FROM node
   WORKDIR /app
   COPY . .
   RUN npm install
   EXPOSE 3000
   CMD ["npm", "start"]
   ```
2. Build and Push the Image:
   ```bash
   docker build -t abrahimcse/web-demo:01 .
   docker login
   docker push abrahimcse/web-demo:01
   ```

### Deploy Web App on Kubernetes

1. Create a Deployment:
   ```bash
   kubectl create deployment my-deployment --image=abrahimcse/web-demo:01
   ```
2. List Deployments and Pods:
   ```bash
   kubectl get deployments
   kubectl get pods
   ```
3. Expose the App:
   ```bash
   kubectl expose deployment my-deployment --type=LoadBalancer --port=3000
   minikube service my-deployment
   ```

---

## Step 5: Updating and Scaling Your App

### Update the App

1. Build and Push the Updated Image:
   ```bash
   docker build -t abrahimcse/web-demo:02 .
   docker push abrahimcse/web-demo:02
   ```
2. Update the Deployment:
   ```bash
   kubectl set image deployment my-deployment web-demo=abrahimcse/web-demo:02
   ```
3. Verify Rollout:
   ```bash
   kubectl rollout status deployment my-deployment
   ```

### Rollback to a Previous Version

```bash
kubectl rollout undo deployment my-deployment
```

### Scale the App

```bash
kubectl scale deployment my-deployment --replicas=4
```

---

## Step 6: Using YAML Files for Configuration

### Deployment YAML File

1. Create a `deployment-node.yml` file using examples from the [Kubernetes API Reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.32/#deployment-v1-apps).
2. Apply the Deployment:
   ```bash
   kubectl apply -f deployment-node.yml
   ```
3. Delete the Deployment:
   ```bash
   kubectl delete -f deployment-node.yml
   ```

### Service YAML File

1. Create a `service-node.yml` file using examples from the [Kubernetes API Reference](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.32/#service-v1-core).
2. Apply the Service:
   ```bash
   kubectl apply -f service-node.yml
   ```
3. Delete the Service:
   ```bash
   kubectl delete -f service-node.yml
   ```

---

## Step 7: Kubernetes Volumes and Multi-Container Apps

### Persistent Volumes

Set up persistent storage for data that needs to persist beyond the lifecycle of a container. Refer to the Kubernetes Volumes Documentation.

### Multi-Container Pods

Deploy multiple containers in a single pod or across separate pods using Kubernetes configuration files.

---

This guide covers the basics to get started with Kubernetes and Minikube. For advanced configurations, consult the [Kubernetes Documentation](https://kubernetes.io/docs/).

