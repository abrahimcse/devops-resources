# Kubernetes Cluster Setup Guide (K8s v1.29 with Calico v3.24.1)

## Table of Contents

1. [Set Static IP (All Nodes)](#1-set-static-ip-all-nodes)
2. [Disable Swap & Set Hostnames (All Nodes)](#2-disable-swap--set-hostnames-all-nodes)
3. [Install Kubernetes Prerequisites (All Nodes)](#3-install-kubernetes-prerequisites-all-nodes)
4. [Initialize Kubernetes Cluster (Master Only)](#4-initialize-kubernetes-cluster-master-only)
5. [Configure Kubectl (Master Only)](#5-configure-kubectl-master-only)
6. [Install Calico CNI Plugin (Master Only)](#6-install-calico-cni-plugin-master-only)
7. [Join Worker Nodes](#7-join-worker-nodes)
8. [Verify Cluster Status (Master)](#8-verify-cluster-status-master)
9. [Deploy Sample App (Master)](#9-deploy-sample-app-master)


---

## 1. Set Static IP (All Nodes)

### Check NIC name:

```bash
ip a
```

### Edit Netplan:

```bash
sudo vim /etc/netplan/50-cloud-init.yaml
```

Example:

```yaml
network:
  version: 2
  ethernets:
    enp0s3:  # Use your actual NIC name
      dhcp4: no
      addresses: [192.168.1.73/24]
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
```

### Disable cloud-init network override:

```bash
sudo vim /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg
```

Add:

```yaml
network: {config: disabled}
```

### Apply Netplan:

```bash
sudo netplan apply
```

---

## 2. Disable Swap & Set Hostnames (All Nodes)

### Disable Swap:

```bash
sudo swapoff -a
sudo vim /etc/fstab  # Comment swap line
```

### Reboot:

```bash
sudo init 6
```

### Set Hostnames:

```bash
# Master Node:
sudo hostnamectl set-hostname master-node

# Worker Nodes:
sudo hostnamectl set-hostname worker-node
```

---

## 3. Install Kubernetes Prerequisites (All Nodes)

### 3.1 Load Kernel Modules:

```bash
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter
```

### 3.2 Configure sysctl:

```bash
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

sudo sysctl --system
```

### 3.3 Install Containerd:

```bash
sudo apt-get update
sudo apt-get install -y containerd
```

### 3.4 Configure Containerd:

```bash
sudo mkdir -p /etc/containerd
sudo containerd config default | sudo tee /etc/containerd/config.toml
sudo sed -i 's/SystemdCgroup = false/SystemdCgroup = true/g' /etc/containerd/config.toml
sudo systemctl restart containerd
sudo systemctl enable containerd
```

### 3.5 Install Kubernetes Tools:
**Reference used for updated version:**
### [Reference](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-using-native-package-management)

```bash
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl

sudo mkdir -p /etc/apt/keyrings

curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.33/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

sudo chmod 644 /etc/apt/keyrings/kubernetes-apt-keyring.gpg 


echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo chmod 644 /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

---

## 4. Initialize Kubernetes Cluster (Master Only)

```bash
sudo kubeadm init \
  --apiserver-advertise-address=192.168.1.73 \
  --pod-network-cidr=192.168.0.0/16 \
  --cri-socket /run/containerd/containerd.sock \
  --ignore-preflight-errors=Swap
```

---

## 5. Configure Kubectl (Master Only)

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

---

## 6. Install Calico CNI Plugin (Master Only)

### [Reference](https://projectcalico.docs.tigera.io/getting-started/kubernetes/self-managed-onprem/onpremises).

Description: In this step, we'll install Calico, a powerful networking solution, to facilitate on-premises deployments in your Kubernetes cluster.

### Install Tigera Operator:

```bash
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.24.1/manifests/tigera-operator.yaml
```

### Download CRDs:

```bash
curl -O https://raw.githubusercontent.com/projectcalico/calico/v3.24.1/manifests/custom-resources.yaml
```

### Apply CRDs:

```bash
kubectl create -f custom-resources.yaml
```

---

## 7. Join Worker Nodes

### Get Join Command (Master):

```bash
kubeadm token create --print-join-command
```

### Run on Worker Node:

```bash
kubeadm join <MASTER_IP>:6443 --token <TOKEN> --discovery-token-ca-cert-hash sha256:<HASH> \
  --cri-socket /run/containerd/containerd.sock
```

---

## 8. Verify Cluster Status (Master)

```bash
kubectl get nodes
kubectl get pods -A
```

---

## 9. Deploy Sample App (Master)

### nginx-pod.yaml

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-test
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80
```

### Apply:

```bash
kubectl apply -f nginx-pod.yaml
```

### Check:

```bash
kubectl get pods
kubectl describe pod nginx-test
kubectl logs nginx-test
```

---

