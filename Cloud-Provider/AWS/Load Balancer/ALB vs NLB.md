# 🆚 ALB vs NLB – কখন কোনটা ব্যবহার করবেন?

এই ডকুমেন্টে সহজ বাংলায় AWS-এর Application Load Balancer (ALB) এবং Network Load Balancer (NLB) এর পার্থক্য ও কখন কোনটা ব্যবহার করবেন, তা ব্যাখ্যা করা হয়েছে।

---

## 📊 তুলনা চার্ট

| বিষয় | ALB (Application Load Balancer) | NLB (Network Load Balancer) |
|------|-------------------------------|-----------------------------|
| **Level (লেভেল)** | Layer 7 (Application Layer) | Layer 4 (Transport Layer) |
| **Protocol** | HTTP, HTTPS | TCP, UDP, TLS |
| **Routing এর ক্ষমতা** | Content-based routing (Path, Hostname অনুযায়ী) | IP/Port অনুযায়ী |
| **Latency (বিলম্ব)** | একটু বেশি | খুব কম (High Performance) |
| **Use Case** | Web app, API Gateway | Real-time app, Database |
| **SSL/TLS Termination** | হ্যাঁ | সীমিত |
| **Health Check** | HTTP/HTTPS Path-based | TCP Port-based |

---

## 🎯 কখন ALB ব্যবহার করবেন?

- যদি URL বা path অনুযায়ী রিকোয়েস্ট route করতে চান
- Web Application, Microservices, API Gateway ব্যবহারের জন্য
- HTTPS certificate ALB handle করুক চান

**উদাহরণ:** `/login`, `/admin`, `/user` path-based routing

✅ ALB ব্যবহার করুন

---

## 🎯 কখন NLB ব্যবহার করবেন?

- Real-time high-performance application এর জন্য
- TCP level application (SSH, Redis, Database, VPN)
- HTTPS termination নিজে handle করবেন

**উদাহরণ:** Redis/Database server, Game server, VPN

✅ NLB ব্যবহার করুন

---

## ✅ সহজ মনে রাখার উপায়

| দরকার যদি হয়... | তাহলে ব্যবহার করুন |
|------------------|---------------------|
| ওয়েব অ্যাপ/ওয়েবসাইট | ALB |
| API Gateway | ALB |
| Database / TCP App | NLB |
| VPN / Game Server | NLB |
| Path-based routing | ALB |
| High speed TCP traffic | NLB |
