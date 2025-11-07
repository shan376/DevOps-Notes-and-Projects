# 🧩 Helm Basics

### 📘 Concept

Helm Kubernetes ka package manager hai jo apps ko **Charts** ke form mein deploy karta hai.
Charts mein YAML templates hoti hain (deployment, service, PVC, etc.) aur aap easily same app ko multiple environments mein deploy kar sakte ho.

### ⚙️ Practical Work (Roman Urdu)

1. **Install Helm**

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
helm version
```

2. **Create Chart**

```bash
helm create mychart
```

3. **Deploy App Using Helm**

```bash
helm install myapp ./mychart
kubectl get all
helm list
```

4. **Assignment: Deploy App with Custom Values**

* Modify `values.yaml`:

```yaml
image:
  repository: nginx
  tag: latest
service:
  type: NodePort
  port: 80
```

* Reinstall Chart:

```bash
helm uninstall myapp
helm install myapp ./mychart
kubectl get svc
```

* Browser Access: `http://<EC2_PUBLIC_IP>:<NodePort>`

### ⚡ Notes

* NodePort range 30000–32667 EC2 security group mein allow hona chahiye.
* Helm release verify karne ke liye `helm list`.

✅ Steps: Helm Installed, Chart Created, App Deployed, Assignment Done.
