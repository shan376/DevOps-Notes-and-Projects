# Argo CD Basics – GitOps Deployment Lab

## 📘 Concept
Argo CD ek GitOps tool hai jo Git repo aur Kubernetes cluster ke beech bridge banata hai.  
Jab Git repo mein YAML changes aate hain, Argo CD automatically cluster mein deploy kar deta hai.  

**Analogy:** Git repo = instruction book, Argo CD = robot chef, Cluster = kitchen.  

---

## 🔧 Lab Environment
- OS: Ubuntu 22.04 LTS (AWS EC2)  
- Kubernetes: Minikube  
- Container Runtime: Docker  
- GitOps Tool: Argo CD  
- GitHub Repo: Personal repo  

---

## ✅ Practical Steps

### 1. Update & Install Docker
```bash
sudo apt update -y
sudo apt install -y docker.io
sudo systemctl enable docker
sudo systemctl start docker
docker --version
````

### 2. Create Non-Root User for Minikube

```bash
sudo adduser minikubeuser
sudo usermod -aG sudo,docker minikubeuser
su - minikubeuser
```

### 3. Install Minikube

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
chmod +x minikube-linux-amd64
sudo mv minikube-linux-amd64 /usr/local/bin/minikube
minikube start
```

### 4. Install kubectl

```bash
sudo snap install kubectl --classic
kubectl version --client
```

### 5. Install Argo CD CLI

```bash
curl -sSL -o argocd https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
chmod +x argocd
sudo mv argocd /usr/local/bin/
argocd version
```

### 6. Deploy Argo CD in Cluster

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl get pods -n argocd
```

### 7. Access Dashboard

```bash
kubectl port-forward svc/argocd-server -n argocd 8081:80
# Open browser: http://localhost:8081
```

### 8. Get Admin Password

```bash
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath="{.data.password}" | base64 -d
```

### 9. Create Sample App

```bash
mkdir -p ~/myn47/manifests
cd ~/myn47/manifests
# deployment.yaml & service.yaml create karo, nginx app define karo
```

### 10. Push to GitHub

```bash
git config --global user.name "shan zafar"
git config --global user.email "shanzafaro46@gmail.com"
cd ~/myn47
git init
git remote add origin https://github.com/shan376/myn47
git add .
git commit -m "Argo CD manifests added"
git branch -M main
git push -u origin main
```

### 11. Login to Argo CD CLI

```bash
argocd login localhost:8081 --insecure
```

### 12. Register GitHub Repo

```bash
argocd repo add https://github.com/shan376/myn47 --username shan376 --password <your-personal-access-token>
```

### 13. Create & Sync App

```bash
argocd app create myn47-app --repo https://github.com/shan376/myn47 --path manifests --dest-server https://kubernetes.default.svc --dest-namespace default
argocd app sync myn47-app
kubectl get pods
```

### 14. Optional Auto-Deploy Test

* Update image in deployment.yaml (e.g., nginx:1.21)
* Commit & push
* Argo CD will auto-sync

---

## 📝 Assignment

* Perform full lab on new EC2
* Take screenshots of deployment, UI, auto-sync logs
* Upload screenshots to GitHub `/screenshots` folder

---

## ✅ Summary

| Task                        | Status |
| --------------------------- | ------ |
| Docker + Minikube setup     | ✅      |
| Non-root user               | ✅      |
| kubectl + Argo CD installed | ✅      |
| Argo CD deployed            | ✅      |
| GitHub repo connected       | ✅      |
| App created & synced        | ✅      |
| Auto-deployment tested      | ✅      |
| Screenshots uploaded        | 🔲     |

```
