## Most Useful K8s Debugging Commands

### 🔎 Logs Management:
```yml
kubectl logs <pod-name> → Pod-এর মেইন কন্টেইনারের লগ দেখা

kubectl logs -f <pod-name> → লাইভ লগ ফলো করা

kubectl logs <pod-name> -c <container> → নির্দিষ্ট কন্টেইনারের লগ

kubectl logs <pod-name> --previous → ক্র্যাশ হওয়ার আগের লগ দেখা

kubectl logs job/<job-name> → কোনো Job-এর লগ বের করা

```
### 📊 Monitoring & Events:
```yml
kubectl top node → নোডের CPU & মেমরি ইউজেজ

kubectl top pod --all-namespaces → সব Pod-এর রিসোর্স ইউজেজ

kubectl get events --all-namespaces → সব ইভেন্ট দেখা

kubectl get events --sort-by='.metadata.creationTimestamp'→টাইমলাইনে ইভেন্ট দেখা

kubectl describe pod <pod> | grep -i "error" → Pod-এর ইভেন্টে ত্রুটি চেক করা
```
### ⚙️ Resources Checking:
```yml
kubectl get pods → বর্তমান নেমস্পেসে সব Pod

kubectl get pods -n <namespace> → নির্দিষ্ট নেমস্পেসের Pod

kubectl get pods -o wide → Pod-এর IP, Node ইত্যাদি বিস্তারিত

kubectl describe pod <pod-name> → Pod-এর ডিটেইলস

kubectl get pod <pod-name> -o yaml → Pod YAML আউটপুট

kubectl get pod <pod-name> -o json → JSON আউটপুট

kubectl get pod <pod-name> -o jsonpath='{.status.containerStatuses[*].state}' → কন্টেইনারের স্টেট

kubectl wait --for=condition=Ready pod/<pod> → Pod Ready হওয়া পর্যন্ত অপেক্ষা
```
### 🌐 Networking & Connectivity:
```yml
kubectl get svc → সব Service

kubectl describe svc <service-name> → নির্দিষ্ট Service-এর বিস্তারিত

kubectl get endpoints → কোন Pod কোন Service-এ কানেক্টেড

kubectl exec -it <pod-name> -- curl <service> → Pod থেকে Service কানেক্টিভিটি টেস্ট

kubectl port-forward <pod-name> <local-port>:<pod-port> → Pod পোর্ট লোকালে ফরোয়ার্ড করা

kubectl get ingress → সব Ingress লিস্ট

kubectl describe ingress <ingress-name> → Ingress কনফিগারেশন

kubectl get networkpolicy → নেটওয়ার্ক পলিসি লিস্ট

kubectl describe networkpolicy <name> → নির্দিষ্ট নেটওয়ার্ক পলিসির বিস্তারিত
```
### 🚀 Deployment & Rollout:
```yml
kubectl get deployments → সব Deployment

kubectl describe deployment <deployment-name> → Deployment ডিটেইলস

kubectl rollout status deployment/<deployment-name> → Rollout স্ট্যাটাস

kubectl rollout history deployment/<deployment-name> → Deployment History

kubectl rollout history deployment/<deployment-name> --revision=2 → নির্দিষ্ট revision এর বিস্তারিত

kubectl rollout undo deployment/<deployment-name> → আগের ভার্সনে রোলব্যাক

kubectl rollout undo deployment/<deployment-name> --to-revision=2 → নির্দিষ্ট revision-এ রোলব্যাক

kubectl rollout restart deployment/<deployment-name> → Deployment রিস্টার্ট

kubectl rollout restart daemonset/<daemonset-name> → DaemonSet রিস্টার্ট

kubectl rollout restart statefulset/<statefulset-name> → StatefulSet রিস্টার্ট

kubectl get rs → ReplicaSet লিস্ট

kubectl describe rs <replicaset-name> → ReplicaSet ডিটেইলস

kubectl get statefulset → StatefulSet লিস্ট

kubectl describe statefulset <name> → StatefulSet বিস্তারিত
```
### 🗄️ Storage & Config:
```yml
kubectl get pv → সব PersistentVolume

kubectl describe pv <pv-name> → PV ডিটেইলস

kubectl get pvc → PVC লিস্ট

kubectl describe pvc <pvc-name> → PVC ডিটেইলস

kubectl get configmap → ConfigMap লিস্ট

kubectl describe configmap <configmap-name> → ConfigMap ডিটেইলস

kubectl get secret → Secret লিস্ট

kubectl describe secret <secret-name> → Secret মেটাডেটা

kubectl get secret <secret-name> -o yaml → Secret ডাটা (encoded)
```
### 🛠️ Others (Useful Utilities):
```yml
kubectl exec -it <pod-name> -- /bin/sh → Pod-এ শেল ওপেন করা

kubectl exec -it <pod-name> -- <command> → নির্দিষ্ট কমান্ড রান করা

kubectl attach -it <pod-name> -c <container> → রানিং কন্টেইনারে Attach হওয়া

kubectl cp <pod-name>:<source-path> <local-path> → Pod থেকে ফাইল কপি করা

kubectl get jobs → Job লিস্ট

kubectl get cronjob → CronJob লিস্ট

kubectl get all → সব রিসোর্স লিস্ট

kubectl config view → কারেন্ট kubeconfig ইনফো

kubectl diff -f <manifest.yaml> → লোকাল vs লাইভ পার্থক্য দেখা

kubectl apply --dry-run=client -f <manifest.yaml> → Apply না করে ভ্যালিডেশন করা

kubectl auth can-i <action> → RBAC পারমিশন চেক

kubectl api-resources → সব API রিসোর্স লিস্ট

kubectl api-versions → সাপোর্টেড API ভার্সন

kubectl get --raw /healthz → Cluster Health চেক

kubectl get --raw /metrics → Cluster Metrics চেক
```

