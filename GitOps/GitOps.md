# GitOps – Full Interview Notes

## 1️⃣ GitOps কী
GitOps = **Git + Operations**  
- **Infrastructure & Application Deployment** সবকিছু **Git repository**-তে কোড আকারে সংরক্ষিত থাকে।  
- সিস্টেমের কাঙ্ক্ষিত অবস্থা (desired state) Git-এ থাকে এবং টুলগুলো (যেমন Argo CD/Flux) সেটাকে cluster বা server-এর সাথে মিলিয়ে রাখে।  
- কোনো পরিবর্তন করতে হলে শুধু Git-এ **commit/push** করলেই deployment আপডেট হয়।

---

## 2️⃣ কেন GitOps ব্যবহার করবেন
| সুবিধা | ব্যাখ্যা |
|--------|---------|
| **Version Control** | সব পরিবর্তন Git commit হিসেবে থাকে, সহজে রোলব্যাক করা যায়। |
| **Audit & Security** | কে কখন কী পরিবর্তন করেছে Git log থেকে দেখা যায়। |
| **Consistency** | Dev, Staging, Production—সব environment একইরকম থাকে। |
| **Automation** | ম্যানুয়ালি kubectl চালানোর দরকার হয় না; টুল অটো-সিঙ্ক করে। |
| **Self-Healing** | কেউ যদি ক্লাস্টারে ম্যানুয়ালি কিছু পাল্টায়, টুল সেটা Git-এর সাথে মিলিয়ে নেয়। |

---

## 3️⃣ মূল ধারণা
- **Declarative Configuration**: YAML/Helm দিয়ে কাঙ্ক্ষিত অবস্থা লিখে দেওয়া হয়।  
- **Git as Single Source of Truth**: Git-ই আসল সত্য।  
- **Continuous Reconciliation**: Operator সবসময় Git-এর অবস্থা cluster-এর সাথে মেলাচ্ছে।

---

## 4️⃣ জনপ্রিয় টুল
- **Argo CD** – Kubernetes-এর জন্য সবচেয়ে বেশি ব্যবহৃত।  
- **Flux CD** – CNCF প্রজেক্ট, হালকা এবং শক্তিশালী।

---

## 5️⃣ GitOps Workflow (Argo CD উদাহরণ)

1. **Repo Setup**  
   - Deployment YAML/Helm chart আলাদা Git repo-তে রাখুন।

2. **Build & Push**  
   - CI pipeline (GitHub Actions/Jenkins) Docker image build করে registry-তে push করবে।

3. **Manifest Update**  
   - নতুন image tag Git repo-তে আপডেট হবে (ম্যানুয়ালি বা automation)।

4. **Argo CD Install & Connect**  
   - Cluster-এ Argo CD ইনস্টল করে Git repo connect করুন।

5. **Auto Sync**  
   - Git-এ commit হলেই Argo CD নতুন state cluster-এ প্রয়োগ করবে।

6. **Rollback**  
   - আগের commit-এ revert করলেই পুরনো state ফিরে আসবে।

---

## 6️⃣ সেরা প্র্যাকটিস
- **Separate Repos**: Application code ও Infrastructure config আলাদা রাখুন।  
- **Branch Strategy**: `main` = production, `staging` = staging।  
- **Image Automation**: Argo Image Updater/Flux Image Automation ব্যবহার করুন।  
- **Access Control**: Cluster-এ direct kubectl access কমিয়ে Git permission ব্যবহার করুন।  
- **Monitoring**: Argo CD/Flux-এর health check ও alert সক্রিয় রাখুন।

---

## 7️⃣ সম্ভাব্য ইন্টারভিউ প্রশ্ন ও উত্তর

### 1️⃣ GitOps কী? DevOps থেকে কীভাবে আলাদা?  
**উত্তর:**  
- **GitOps** হলো একটি পদ্ধতি যেখানে **Git repository-ই একমাত্র source of truth** হিসেবে কাজ করে।  
- আপনার Infrastructure ও Application deployment-এর সব configuration (YAML, Helm chart ইত্যাদি) Git-এ রাখা হয়।  
- একটি GitOps টুল (যেমন Argo CD/Flux) স্বয়ংক্রিয়ভাবে Git-এর অবস্থা cluster/server-এর সাথে মেলাতে থাকে।  

**DevOps বনাম GitOps:**  
- DevOps হল একটি বিস্তৃত কালচার ও প্র্যাকটিস (CI/CD, automation, collaboration)।  
- GitOps হল সেই DevOps-এর ভেতরের একটি নির্দিষ্ট মডেল যা **deployment এবং configuration management-এ Git-কে কেন্দ্র করে কাজ করে।**  
- সহজভাবে: *“সব GitOps DevOps-এর অংশ, কিন্তু সব DevOps GitOps নয়।”*

### 2️⃣ GitOps-এর মূল সুবিধা কী কী?  
**উত্তর:**  
- **Version Control:** সব পরিবর্তন Git commit হিসেবেই থাকে, ইতিহাস দেখা সহজ।  
- **Easy Rollback:** পুরনো commit-এ ফিরে গেলেই আগের অবস্থা ফিরে আসে।  
- **Consistency:** Dev, Staging, Production—সব environment একই কনফিগে থাকে।  
- **Automation:** ম্যানুয়ালি kubectl apply করার দরকার নেই; টুল নিজেই সিঙ্ক করে।  
- **Audit & Security:** কে কখন কী পরিবর্তন করেছে Git log থেকেই জানা যায়।  
- **Self-healing:** কেউ ম্যানুয়ালি কিছু পাল্টালে টুল আবার Git-এর অবস্থা ফিরিয়ে আনে।

### 3️⃣ Argo CD কীভাবে কাজ করে?  
**উত্তর:**  
- Argo CD একটি Kubernetes-native GitOps টুল।  
- এটি নির্দিষ্ট Git repo ক্রমাগত watch করে।  
- Git-এ কোনো পরিবর্তন (নতুন commit) হলেই Argo CD Kubernetes cluster-এ সেই পরিবর্তন apply করে।  
- যদি cluster-এ Git থেকে আলাদা কোনো পরিবর্তন হয়, Argo CD সেটিকে আবার Git-এর অবস্থায় ফিরিয়ে আনে।  
- এতে **auto-sync, web UI, CLI, RBAC, health check** ইত্যাদি সুবিধা আছে।

### 4️⃣ GitOps-এ রোলব্যাক কীভাবে করবেন?  
**উত্তর:**  
- যেই Git commit থেকে সমস্যা শুরু হয়েছে, সেটিকে revert বা পুরনো commit-এ checkout করুন।  
- Git repo আগের অবস্থায় ফিরলেই Argo CD/Flux সেই পুরনো কনফিগ আবার cluster-এ প্রয়োগ করে দেবে।  
- আলাদা কোনো ম্যানুয়াল kubectl কমান্ডের প্রয়োজন হয় না।

### 5️⃣ Self-healing বলতে কী বোঝায়?  
**উত্তর:**  
- যদি কেউ সরাসরি cluster-এ ম্যানুয়ালি কিছু পরিবর্তন করে (যেমন pod replica সংখ্যা পাল্টায়), GitOps টুল তা detect করে এবং Git-এ সংরক্ষিত configuration অনুযায়ী আবার সঠিক অবস্থায় ফিরিয়ে আনে।  
- এই অটোমেটিক ঠিক করার প্রক্রিয়াকেই self-healing বলা হয়।

### 6️⃣ GitOps-এর workflow বোঝান।  
**উত্তর (ধাপ আকারে):**  
1. **Developer Commit** – YAML/Helm ইত্যাদির মাধ্যমে desired state Git-এ push করা হয়।  
2. **Git Repository** – single source of truth।  
3. **GitOps Operator (Argo CD/Flux)** – Git repo observe করে।  
4. **Sync to Cluster** – Git-এ পরিবর্তন detect করে cluster-এ apply করে।  
5. **Continuous Reconciliation** – কোনো drift হলে আবার Git-এর সাথে মিলিয়ে ফেলে।

### 7️⃣ CI/CD এর মধ্যে GitOps-এর role কী?  
**উত্তর:**  
- CI (Continuous Integration) কোড build, test এবং artifact (যেমন Docker image) তৈরি করে।  
- GitOps মূলত CD (Continuous Delivery/Deployment)-এর অংশ।  
- CI pipeline নতুন image push করার পর manifest-এ image tag update হয় এবং Git-এ commit হয়।  
- GitOps টুল (Argo CD/Flux) সেই commit detect করে এবং স্বয়ংক্রিয়ভাবে নতুন ভার্সন deploy করে।  
- অর্থাৎ, **GitOps হল CD ধাপের স্বয়ংক্রিয় ও Git-কেন্দ্রিক বাস্তবায়ন।**


