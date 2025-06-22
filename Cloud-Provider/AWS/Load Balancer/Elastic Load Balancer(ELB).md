# Elastic Load Balancer (ELB) in AWS

## 1. What is a Load Balancer?

A Load Balancer is a service that **distributes incoming traffic** across multiple servers.This helps to avoid overload on any single server and keeps your application available.

---

![ELB](https://github.com/abrahimcse/devops-resources/blob/main/Cloud-Provider/AWS/Images/ELB.png)

## 2. Why use ELB?

| Reason              | Explanation                                               |
|---------------------|-----------------------------------------------------------|
| 🔁 High Availability | If one server fails, ELB sends traffic to other servers   |
| ⚡ Scalability       | Works well with Auto Scaling                              |
| 🛡️ Security         | Supports HTTPS, SSL certificates, and AWS WAF            |
| 📊 Monitoring       | Can monitor traffic, errors, and latency via CloudWatch  |

---

## 3. Types of ELB
- A. Application Load Balancer (ALB)
- B. Network Load Balancer (NLB)
- C. Classic Load Balancer (CLB)

### A. Application Load Balancer (ALB)
- Works at **Layer 7** (Application Layer)
- Best for **HTTP/HTTPS** traffic
- Supports path-based routing (e.g., `/api`, `/images`)
- Supports host-based routing (different domains)
- Supports WebSocket, HTTP/2, and integrates with AWS WAF

### B. Network Load Balancer (NLB)
- Works at **Layer 4** (Transport Layer)
- Best for **TCP/UDP** traffic
- Can handle millions of requests per second
- Supports static IP addresses
- Preserves the source IP address

### C. Classic Load Balancer (CLB)
- Works at Layer 4 & 7 (Legacy)
- Basic load balancing
- For old EC2 instances (EC2-Classic)
- Not recommended for new projects

---

## 4.🧠 AWS Elastic Load Balancer (ELB) Key Concepts

### 🌐 4.1. Resolve DNS Name

* Every AWS Load Balancer has a **DNS name** like:
  `myapp-1234567890.elb.amazonaws.com`
* When a client requests this name, **DNS resolves** it to an **IP address** of the Load Balancer.
* The client connects to that IP and sends traffic.

---

### 🗱 4.2. Load Balancer

* A service that **distributes incoming traffic** to multiple targets (EC2, IPs, etc.).
* Increases **scalability**, **availability**, and **fault tolerance**.
* Types:

  * **ALB (Application Load Balancer)** – HTTP/HTTPS (Layer 7)
  * **NLB (Network Load Balancer)** – TCP/UDP (Layer 4)
  * **CLB (Classic Load Balancer)** – Legacy

---

### ⚓ 4.3. Listener

* A **listener checks for incoming connections** on a specific protocol and port.
* Example:

  * HTTP on port 80
  * HTTPS on port 443
* Listener has **rules** to forward traffic to different **target groups**.

---

### 🎯 4.4. Target Group

* A logical group that contains **targets** like EC2, IPs, or Lambda functions.
* Each group has:

  * Protocol and port (e.g., HTTP:80)
  * Health check settings
* Listener forwards requests to **one or more target groups**.

---

### 👨‍💻 4.5. Target

* The **actual resource** (like an EC2 instance) that handles traffic.
* Must be **registered** in a target group.
* Targets are monitored using **health checks**.

---

### 🌐 4.6. Subnet

* A **range of IPs inside a VPC**.
* Load Balancer must be created in **at least two subnets in different Availability Zones** to ensure high availability.
* Example:

  * Subnet-A: us-east-1a
  * Subnet-B: us-east-1b

---

### ❤️ 4.7. Health

* The **status** of a target:

  * ✅ Healthy: Receiving traffic
  * ❌ Unhealthy: Failing health checks
  * 🔄 Initial: Just added, under checking
  * 🚩 Draining: Removing from service

---

### 🦥 4.8. Health Check

* ELB sends periodic requests to targets to **test if they are working**.
* Based on:

  * Protocol: HTTP, HTTPS, TCP
  * Path: e.g., `/health`
  * Port: e.g., 80
  * Interval: Time between checks (default 30s)
  * Timeout: Max wait for response (default 5s)
  * Thresholds: # of successes/failures to be marked healthy/unhealthy

---

### ❌ 4.9. Unhealthy Check

* If a target fails multiple checks in a row (e.g., 3 times), it's marked **Unhealthy** and **removed** from traffic rotation.

---

### 🕒 4.10. Period Time / Interval

* Time **between two health check requests**.
* Example:

  * Interval = 30s
  * Unhealthy threshold = 3
    → After 90 seconds of failure → marked unhealthy

---

### ⏱️ 4.11. Response Time

* How long it takes the target to **respond to the health check**.
* If it responds after the timeout (e.g., 5s), it's considered a **failure**.

---

### 🔀 4.12. Cross-Zone Load Balancing

* When **cross-zone is enabled**, the Load Balancer **can send traffic to any healthy target in any AZ**, **regardless of the source AZ**.
* ✅ Ensures even distribution
* ❌ Without it, traffic from one zone goes only to targets in the same zone

**Best Practice: Always enable it for balanced load.**


## 5. How does ALB work?

> Client → ALB (checks routing rules) → Target Group → EC2 Instances

- ALB checks request path or host to decide where to send traffic  
- Sends traffic to the right target group  
- Only healthy targets receive traffic

---

## 6. When to use which Load Balancer?

| Use Case                   | Load Balancer           |
|----------------------------|------------------------|
| Web apps (HTTP/HTTPS)       | ALB                    |
| Microservices               | ALB                    |
| TCP/UDP apps (Gaming, IoT)  | NLB                    |
| High performance needed     | NLB                    |
| Legacy EC2 apps             | CLB (avoid if possible) |

---

## 7. Step-by-step to create an Application Load Balancer

1. **Create Load Balancer**  
   - Go to EC2 Console → Load Balancers → Create Load Balancer  
   - Select **Application Load Balancer**  
   - Enter name, choose VPC and subnets  

2. **Configure Listener & Protocol**  
   - Select HTTP or HTTPS  
   - Add SSL certificate if HTTPS  

3. **Configure Security Groups**  
   - Create or select a security group allowing HTTP/HTTPS  

4. **Create Target Group**  
   - Choose target type (Instance, IP, Lambda)  
   - Set protocol (HTTP) and port (80 or 443)  
   - Set health check path `/`  

5. **Register Targets**  
   - Select EC2 instances or targets  

6. **Review and Create**  
   - Check all settings and click **Create**

---

## 8. Monitoring ELB

Use **CloudWatch Metrics** like:  
- HealthyHostCount  
- RequestCount  
- Latency  
- HTTPCode_ELB_4XX / 5XX errors

---

## 9. Common Interview Questions

- What types of ELB are there?  
- Difference between ALB and NLB?  
- How does ALB route requests?  
- What is health check and how does it work?  
- Which load balancer to choose for static IP?

---

## 10. Quick Summary

| Load Balancer | Layer | Traffic Type   | Best Use Case               |
|---------------|-------|----------------|----------------------------|
| ALB           | 7     | HTTP/HTTPS     | Web apps, APIs, Microservices |
| NLB           | 4     | TCP/UDP        | Gaming, IoT, High speed apps  |
| CLB           | 4/7   | Legacy traffic | Old EC2 apps only             |

---

## Conclusion:

- Use **ALB** for web applications and APIs  
- Use **NLB** for TCP/UDP and very high performance needs  
- Avoid **CLB** for new projects, only for legacy support  

---
