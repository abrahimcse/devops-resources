# Kubernetes Node Rejoin Guide

This document outlines the steps to safely remove a node from a Kubernetes cluster, modify it (e.g., rename or reconfigure), and then rejoin it to the cluster.

---

## 📍 Step 1: Delete Node from Master (Control Plane)

### Run on Master Node:

```bash
kubectl get nodes          # Get current node list
kubectl delete node <node-name>
```

**Example:**

```bash
kubectl delete node devops
```

---

## 🖥️ Step 2: Modify the Worker Node

### On the Worker Node:

1. **Change the hostname (if needed):**

```bash
sudo hostnamectl set-hostname worker-2
```

2. **Reboot the node (recommended):**

```bash
sudo reboot
```

---

## 🧹 Step 3: Reset Kubernetes on Worker Node

Before rejoining, ensure all previous Kubernetes configurations are removed.

```bash
sudo kubeadm reset -f
sudo ip link delete cni0       # Optional: delete old CNI bridge
sudo ip link delete flannel.1  # Optional: delete old flannel
sudo systemctl restart containerd
sudo systemctl restart kubelet
```

---

## 🔗 Step 4: Rejoin the Cluster

### From the Master Node, generate a new join command:

```bash
kubeadm token create --print-join-command
```

### Example Output:

```bash
kubeadm join 192.168.1.72:6443 --token abcdef.0123456789abcdef \
    --discovery-token-ca-cert-hash sha256:<hash> \
    --cri-socket /run/containerd/containerd.sock
```

### Run this command on Worker Node:

```bash
sudo kubeadm join 192.168.1.72:6443 --token abcdef.0123456789abcdef \
    --discovery-token-ca-cert-hash sha256:<hash> \
    --cri-socket /run/containerd/containerd.sock
```

---

## ✅ Step 5: Confirm the Node is Ready

### On Master Node:

```bash
kubectl get nodes
```

You should now see `worker-2` or your modified node name in `Ready` state.

---

## 📌 Notes:

* Ensure correct hostname before join.
* If kubelet fails to start, check logs: `sudo journalctl -u kubelet -xe`.
* The Pod CIDR should be same across the cluster.
* Don’t forget to restart container runtime (like containerd) before joining.

