# AWS ALB (Application Load Balancer) Creation Guide

A simple and sequential guide to creating an Application Load Balancer (ALB) in AWS.

---

## ✅ Prerequisites

- At least **2 EC2 instances** running in the **same VPC**.
- EC2 instances must be in **the same availability zone(s)**.
- Security groups for instances must allow **HTTP (port 80)** inbound traffic.

---

## 🧭 Step-by-Step Instructions

### 1. Go to EC2 Console
- AWS Console > **EC2** > Left sidebar > **Load Balancers**

### 2. Create Load Balancer
- Click **“Create Load Balancer”**
- Choose **Application Load Balancer**
- Click **“Create”**

### 3. Configure Load Balancer
- **Name**: `my-alb` (or any name you prefer)
- **Scheme**: `internet-facing`
- **IP address type**: `IPv4`
- **Listeners**: HTTP (port 80)
- **Availability Zones**:  
  - Select your **VPC**  
  - Choose **2 subnets in different AZs**

### 4. Configure Security Group
- Choose an existing Security Group OR create a new one:
  - Inbound: Allow **HTTP (port 80)** from `0.0.0.0/0`
  - Outbound: Default (allow all)

### 5. Configure Target Group
- **Target type**: `Instance`
- **Name**: `my-target-group`
- **Protocol**: HTTP
- **Port**: 80
- **Health check path**: `/`

### 6. Register Targets
- Select your EC2 instances
- Click **Add to registered**
- Click **Next**

### 7. Review and Create
- Review your configuration
- Click **“Create Load Balancer”**

---

## ✅ Verify the Setup

- Go to **EC2 > Load Balancers**
- Copy the **DNS name** of your ALB
- Paste it into your browser  
- You should see the response from one of your EC2 instances.

---

## ℹ️ Notes

- Health checks must return **HTTP 200** for instance to be considered healthy.
- Ensure EC2 web servers are listening on **port 80**.

---

