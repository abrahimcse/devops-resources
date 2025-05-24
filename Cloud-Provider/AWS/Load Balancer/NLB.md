# AWS NLB (Network Load Balancer) Creation Guide

A clear, simple, step-by-step guide to creating a **Network Load Balancer (NLB)** in AWS.

---

## ✅ Prerequisites

- At least **2 EC2 instances** running in the **same VPC**
- EC2 instances should be in **the same or multiple availability zones**
- Instances should have **services running on TCP ports** (e.g., port 80, 443, 22, etc.)
- Security group must allow **inbound traffic on the intended port**

---

## 🧭 Step-by-Step Instructions

### 1. Go to EC2 Console
- AWS Console > **EC2** > Left sidebar > **Load Balancers**

### 2. Create Load Balancer
- Click **“Create Load Balancer”**
- Choose **Network Load Balancer**
- Click **“Create”**

### 3. Configure Load Balancer
- **Name**: e.g., `my-nlb`
- **Scheme**: `internet-facing` or `internal`
- **IP address type**: IPv4
- **Listeners**:
  - Protocol: TCP
  - Port: 80 (or your application's port)
- **Availability Zones**:
  - Select your **VPC**
  - Choose **1 or more subnets in different AZs**

### 4. Configure Target Group
- **Target group name**: e.g., `nlb-target-group`
- **Target type**: `Instance`
- **Protocol**: TCP
- **Port**: 80 (or match your backend service port)
- **Health checks**:
  - Protocol: TCP
  - Port: traffic port (default)

### 5. Register Targets
- Select the EC2 instances running your service
- Click **Add to registered**
- Click **Next**

### 6. Review and Create
- Review all settings
- Click **“Create Load Balancer”**

---

## ✅ Verify the Setup

- Go to **EC2 > Load Balancers**
- Copy the **DNS name** of your NLB
- Open a terminal or browser (if HTTP)
  - For example: `curl http://<NLB-DNS>` or `telnet <NLB-DNS> 80`
- You should see the response from one of your backend EC2s

---

## ℹ️ Notes

- NLB works at **Layer 4 (Transport Layer)** — handles raw TCP traffic
- Best suited for high performance, low-latency apps (e.g., database, backend services)
- Does **not support** content-based routing like ALB

---

