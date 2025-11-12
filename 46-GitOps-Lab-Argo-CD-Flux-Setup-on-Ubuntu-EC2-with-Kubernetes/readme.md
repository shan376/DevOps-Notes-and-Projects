# GitOps Lab – Argo CD & Flux Setup on Kubernetes

## 📘 Concept
GitOps ek DevOps practice hai jisme infrastructure aur app deployments Git repo me store hotay hain.  
GitOps tools (Argo CD / Flux) automatically Git changes detect karke Kubernetes cluster me deploy kar dete hain.  
➡️ Git = Source of Truth, Deployment = Automated from Git.

## 🧁 Real-Life Example
Bakery example:  
- Recipe Book = Git Repo  
- Chef = Argo CD / Flux  
- Bakery = Kubernetes Cluster  
- Recipe Update = Git Changes  
- Product = App deployed

## ✅ Lab Environment
- OS: Ubuntu 22.04 LTS  
- Platform: AWS EC2  
- Kubernetes: Minikube  
- Container: Docker  
- GitOps Tool: Argo CD (main), Flux (optional)  
- Access: SSH via PuTTY

## 🟢 Practical Steps

1. **Install Docker**  
```bash
sudo apt update -y
sudo apt install -y docker.io
sudo systemctl start docker
sudo systemctl enable docker
docker --version
````

2. **Install Minikube**

* Download & install, create non-root user `minikubeuser`
* Start Minikube as non-root

3. **Install kubectl CLI**

```bash
sudo snap install kubectl --classic
kubectl version --client
```

4. **Install Argo CD CLI**

```bash
curl -sSL https://github.com/argoproj/argo-cd/releases/download/v2.4.4/argocd-linux-amd64 -o argocd
chmod +x argocd
sudo mv argocd /usr/local/bin/
argocd version
```

5. **Deploy Argo CD in Kubernetes**

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl port-forward svc/argocd-server -n argocd 8081:80
```

* Login: `admin` / password from secret

6. **Connect GitHub Repo**

```bash
argocd login localhost:8081 --insecure
argocd repo add https://github.com/shan376/myn46 --username shan376 --password <TOKEN>
```

7. **Create & Sync App in Argo CD**

* Create manifests (deployment.yaml + service.yaml)
* Git init, commit & push
* Argo CD app create & sync

```bash
argocd app create myn46-app --repo https://github.com/shan376/myn46 --path manifests --dest-server https://kubernetes.default.svc --dest-namespace default
argocd app sync myn46-app
kubectl get pods
```

8. **Test GitOps Deployment**

* Update deployment.yaml (e.g., image version)
* Git commit & push
* Argo CD automatically redeploys

9. **Optional: Install Flux**

```bash
curl -s https://fluxcd.io/install.sh | sudo bash
flux --version
flux create source git my-app-repo --url=https://github.com/shan376/myn46 --branch=main
flux create kustomization my-app --source=GitRepository/my-app-repo --path="./" --prune=true --interval=10m
```

10. **Cleanup**

```bash
argocd app delete my-app --cascade
kubectl delete namespace argocd
flux delete kustomization my-app
```

## 📌 GitOps Flow

GitHub Repo → Argo CD / Flux → Kubernetes Cluster → Auto Deployment

## 📝 Assignment

* Install Docker & Minikube ✅
* Setup non-root user ✅
* Install kubectl & Argo CD CLI ✅
* Deploy Argo CD in Minikube ✅
* Connect GitHub Repo ✅
* Sync app & test GitOps flow ✅
* Optional: Flux 🔲
* Upload YAML & screenshots to GitHub 🔲

```
