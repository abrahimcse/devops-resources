# Argo CD – Full Concept & Interview Notes

## 1️⃣ Argo CD কী
- **Argo CD** হল Kubernetes-এর জন্য একটি **GitOps Continuous Delivery (CD) টুল**।  
- এটি একটি **declarative, GitOps-based** ডেপ্লয়মেন্ট মেকানিজম।  
- Git repository তে সংরক্ষিত Kubernetes manifest বা Helm chart অনুযায়ী cluster-এর অবস্থা সবসময় sync করে রাখে।

---

## 2️⃣ মূল বৈশিষ্ট্য
- **Declarative Setup**: সমস্ত configuration Git repo-তে থাকে।
- **Git as Source of Truth**: Git repo-র state-ই আসল সত্য।
- **Automated Sync**: Git এ commit হলেই Argo CD cluster আপডেট করে।
- **Self-healing**: কেউ cluster-এ ম্যানুয়ালি কিছু পাল্টালে Git অনুযায়ী আবার ঠিক করে দেয়।
- **Multi-Cluster Support**: একাধিক Kubernetes cluster একসাথে ম্যানেজ করা যায়।
- **Web UI & CLI**: সুন্দর UI এবং শক্তিশালী CLI উভয়ই আছে।
- **RBAC & SSO**: Role Based Access Control এবং Single Sign-On সাপোর্ট।

---

## 3️⃣ Architecture / Components
1. **API Server** – REST/GRPC API সরবরাহ করে, UI/CLI এখানেই সংযোগ করে।  
2. **Repository Server** – Git repo থেকে manifests/Helm chart ফেচ করে।  
3. **Application Controller** – Git এবং cluster-এর অবস্থা তুলনা করে, drift থাকলে sync করে।  
4. **Dex (Optional)** – Authentication/SSO হ্যান্ডেল করতে ব্যবহৃত হয়।

---

## 4️⃣ কাজের ধাপ (Workflow)
1. **Developer Commit** – Kubernetes YAML/Helm chart Git repo তে push।  
2. **Argo CD Watch** – নির্দিষ্ট repo branch ক্রমাগত পর্যবেক্ষণ।  
3. **Sync** – নতুন commit detect করলে cluster-এর সাথে মেলানো।  
4. **Health & Status** – UI/CLI দিয়ে app-এর অবস্থা ও health দেখা।  
5. **Rollback** – পুরনো commit এ revert করলেই আগের config প্রয়োগ।

---

## 5️⃣ ইনস্টলেশন (সংক্ষিপ্ত ধাপ)
```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
- তারপর LoadBalancer/Ingress দিয়ে UI expose করুন।  
- `argocd` CLI ইনস্টল করে Git repo connect করুন।

---

## 6️⃣ সেরা প্র্যাকটিস
- **Separate Git Repos** – App code এবং Deployment config আলাদা রাখুন।  
- **Branch Strategy** – main = production, develop = staging।  
- **RBAC & SSO** – প্রপার অ্যাক্সেস কন্ট্রোল।  
- **Monitoring & Alerting** – Argo CD health checks ও notifications সক্রিয় করুন।  
- **Automation** – Image updater ব্যবহার করে নতুন image tag স্বয়ংক্রিয়ভাবে update করুন।

---

## 7️⃣ সম্ভাব্য ইন্টারভিউ প্রশ্ন ও উত্তর

### 1️⃣ Argo CD কী?
**উত্তর:** Argo CD হলো Kubernetes-এর জন্য একটি declarative GitOps CD টুল যা Git repo-তে রাখা Kubernetes manifests/Helm chart অনুযায়ী cluster সবসময় sync করে রাখে এবং স্বয়ংক্রিয় deployment নিশ্চিত করে।

### 2️⃣ Argo CD কীভাবে কাজ করে?
**উত্তর:**  
- Argo CD Git repo-কে source of truth ধরে।  
- নতুন commit হলে Repository Server manifest নিয়ে আসে।  
- Application Controller cluster-এর সাথে মিলিয়ে দেখে এবং drift থাকলে sync করে।  
- UI/CLI এর মাধ্যমে health/status দেখা ও ম্যানেজ করা যায়।

### 3️⃣ Argo CD-তে Self-healing কীভাবে হয়?
**উত্তর:** Cluster-এ কেউ ম্যানুয়ালি কোনো পরিবর্তন করলে Argo CD Git-এর কনফিগ অনুযায়ী স্বয়ংক্রিয়ভাবে পুরনো সঠিক অবস্থায় ফিরিয়ে আনে।

### 4️⃣ Argo CD আর Jenkins এর পার্থক্য কী?
**উত্তর:**  
- Jenkins মূলত CI (build/test) এর জন্য ব্যবহৃত হয় এবং CD করতে স্ক্রিপ্টিং লাগে।  
- Argo CD বিশেষভাবে Kubernetes-এর জন্য GitOps CD টুল, যা স্বয়ংক্রিয়ভাবে Git repo-এর সাথে cluster sync করে।

### 5️⃣ Argo CD-তে Application Controller এর ভূমিকা কী?
**উত্তর:** Application Controller Git repo থেকে পাওয়া desired state এবং cluster-এর current state তুলনা করে, যদি পার্থক্য থাকে তবে sync করে বা alert দেয়।

### 6️⃣ Multi-Cluster deployment কিভাবে করবেন?
**উত্তর:** Argo CD একাধিক Kubernetes cluster কে manage করতে পারে। প্রতিটি cluster-কে credential সহ Argo CD-তে যোগ করে আলাদা Application object দিয়ে ম্যানেজ করা হয়।

### 7️⃣ CI/CD pipeline এ Argo CD এর জায়গা কোথায়?
**উত্তর:** CI (যেমন GitHub Actions/Jenkins) কোড build ও image push করে।  
CD অংশে Argo CD Git repo তে নতুন commit detect করে Kubernetes cluster-এ স্বয়ংক্রিয়ভাবে deploy করে।

### 8️⃣ Rollback কিভাবে করবেন?
**উত্তর:** পুরনো commit-এ Git repo revert করুন। Argo CD সেটাকে detect করে cluster আগের অবস্থায় ফিরিয়ে দেবে।

### 9️⃣ Argo CD এর নিরাপত্তা (Security) কিভাবে নিশ্চিত করবেন?
**উত্তর:**  
- RBAC enable করুন।  
- SSO/Dex ব্যবহার করুন।  
- Git repo-র proper branch protection রাখুন।  
- TLS/Ingress সঠিকভাবে configure করুন।

### 🔟 Argo CD Notifications কি?
**উত্তর:** এটি deployment status, sync success/failure ইত্যাদির জন্য Slack/Email/Webhook নোটিফিকেশন পাঠানোর ফিচার।

---

## 8️⃣ এক লাইনে
**“Argo CD = GitOps CD টুল, যা Git-এর config কে Kubernetes cluster এর সাথে স্বয়ংক্রিয়ভাবে মিলিয়ে রাখে।”**