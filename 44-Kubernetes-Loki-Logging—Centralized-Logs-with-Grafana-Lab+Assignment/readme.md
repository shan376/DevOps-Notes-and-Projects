# Kubernetes Logging with Loki — Full Setup from Scratch

## 📘 Concept
Loki ek centralized log aggregation system hai, specially Kubernetes ke liye. Ye logs ko efficiently collect, store aur query karta hai. Prometheus metrics ke liye hota hai, Loki logs ke liye.  

### ❓ Why Loki?
- Centralized logs: Sab pods ka logs ek jagah
- Real-time logs: SSH ki zarurat nahi
- Query & filter support: Pod, container ya error level filter
- Lightweight: Promtail ke sath easily integrate

### 🧵 Real-Life Example
Jaise factory mein central CCTV room hai jahan sab machines ka live feed hai, waise hi Loki sab pods ka logs Grafana se dikha deta hai.

---

## 🧰 Lab Work (Practical Steps)

### 1️⃣ Launch EC2 Ubuntu Server
- OS: Ubuntu 22.04 LTS, Type: t2.medium  
- Open ports: 22, 3000 (Grafana), 3100 (Loki optional)

### 2️⃣ Install Docker & Kubernetes
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y docker.io containerd kubelet kubeadm kubectl
sudo systemctl enable docker containerd
sudo systemctl start docker containerd
sudo swapoff -a
sudo modprobe overlay br_netfilter
````

### 3️⃣ Initialize Cluster

```bash
sudo kubeadm init --pod-network-cidr=192.168.0.0/16
mkdir -p $HOME/.kube
sudo cp /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

### 4️⃣ Install Calico CNI

```bash
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```

### 5️⃣ Install Helm

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
helm version
```

### 6️⃣ Deploy Loki Stack

```bash
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
kubectl taint nodes --all node-role.kubernetes.io/control-plane-
kubectl create namespace logging
helm install loki-stack grafana/loki-stack \
  --namespace=logging \
  --set grafana.enabled=true \
  --set prometheus.enabled=false \
  --set loki.persistence.enabled=false \
  --set promtail.enabled=true \
  --wait --timeout 10m
```

### 7️⃣ Verify Pods

```bash
kubectl get pods -n logging
```

* Ensure: loki-stack-promtail, loki-stack-grafana, loki-stack-loki running

### 8️⃣ Access Grafana

```bash
kubectl port-forward svc/loki-stack-grafana 3000:80 -n logging
```

* Browser: [http://localhost:3000](http://localhost:3000)
* Login:

```bash
kubectl get secret --namespace logging loki-stack-grafana -o jsonpath="{.data.admin-password}" | base64 -d
Username: admin
Password: (above output)
```

### 9️⃣ Explore Logs

* Go “Explore” tab → Source: Loki
* Queries:

```text
{job="kubernetes-pods"}
{pod=~".*calico.*"}
{container=~".*coredns.*"}
```

---

## 🎯 Assignment

1. Login Grafana at [http://localhost:3000](http://localhost:3000)
2. Explore logs & filter using queries above
3. Save screenshot of logs
4. 🔥 Bonus: Create alert for logs with level=error

---

**Outcome:** Centralized Kubernetes logging ready, real-time monitoring aur filtering Grafana ke zariye ✅

```
