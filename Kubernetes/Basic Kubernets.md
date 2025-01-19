# **Kubernetes: Container Orchestration Platform**

## **What is Kubernetes?**
**Kubernetes (K8s)** is an open-source platform for managing containerized applications across clusters of machines. It automates the deployment, scaling, and operations of application containers.

- Developed by **Google** in 2014 and donated to the **Cloud Native Computing Foundation (CNCF)**.  
- Written in **Go** language.  
- Supports various container runtimes, including Docker, containerd, and CRI-O.

---

## **Key Features of Kubernetes**
- **Automated Deployment**: Simplifies application deployment.  
- **Scaling**: Automatically adjusts resource allocation based on demand.  
- **Self-Healing**: Restarts failed containers and replaces unhealthy nodes.  
- **Load Balancing**: Efficiently distributes traffic across Pods.  
- **Rolling Updates**: Deploys updates with zero downtime.  
- **Storage Orchestration**: Supports dynamic storage provisioning.

---

## **Kubernetes Architecture**

### **Key Components**:
1. **Master Node (Control Plane)**:  
   - **API Server**: Exposes Kubernetes APIs.  
   - **Scheduler**: Assigns workloads (Pods) to nodes.  
   - **Controller Manager**: Manages cluster state and replication.  
   - **etcd**: Key-value store for cluster configuration.

2. **Worker Node**:  
   - **Kubelet**: Manages container lifecycle on the node.  
   - **Kube-Proxy**: Handles networking and load balancing.  
   - **Container Runtime**: Runs containers (e.g., Docker, containerd).

3. **Pods**: The smallest deployable unit containing one or more containers.

---

## **Kubernetes Commands**

### **Cluster Management**
1. **Check Kubernetes version**:  
   ```bash
   kubectl version
