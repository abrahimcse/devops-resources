# Elastic Load Balancer (ELB) in AWS

## 1. What is a Load Balancer?

A Load Balancer is a service that **distributes incoming traffic** across multiple servers.This helps to avoid overload on any single server and keeps your application available.

---

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

## 4. How does ALB work?

> Client → ALB (checks routing rules) → Target Group → EC2 Instances

- ALB checks request path or host to decide where to send traffic  
- Sends traffic to the right target group  
- Only healthy targets receive traffic

---

## 5. When to use which Load Balancer?

| Use Case                   | Load Balancer           |
|----------------------------|------------------------|
| Web apps (HTTP/HTTPS)       | ALB                    |
| Microservices               | ALB                    |
| TCP/UDP apps (Gaming, IoT)  | NLB                    |
| High performance needed     | NLB                    |
| Legacy EC2 apps             | CLB (avoid if possible) |

---

## 6. Step-by-step to create an Application Load Balancer

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

## 7. Monitoring ELB

Use **CloudWatch Metrics** like:  
- HealthyHostCount  
- RequestCount  
- Latency  
- HTTPCode_ELB_4XX / 5XX errors

---

## 8. Common Interview Questions

- What types of ELB are there?  
- Difference between ALB and NLB?  
- How does ALB route requests?  
- What is health check and how does it work?  
- Which load balancer to choose for static IP?

---

## 9. Quick Summary

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
