# Kubernetes Ingress Lab – Ubuntu 22.04

## 📘 Concept

Ingress Kubernetes object hai jo external HTTP/HTTPS traffic ko services tak route karta hai.

* Path-based ya host-based routing (/, /api, /admin)
* Ingress Controller (e.g., NGINX) rules process karta hai

### 🧠 Roman Urdu

Ingress ek traffic controller hai jo decide karta hai ke request kis service ko jaye.
Ek IP par multiple services alag paths se run kar sakte ho.

### 🔁 Example

* / → frontend
* /api → backend
* /admin → admin panel

---

## 🔧 Lab Steps

### 1. Update & Prerequisites

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y ca-certificates curl gnupg lsb-release
```

### 2. Docker Install

```bash
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
sudo systemctl enable docker && sudo systemctl start docker
```

### 3. Kubernetes Repo & Tools

```bash
sudo apt install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

### 4. Disable Swap

```bash
sudo swapoff -a
sudo sed -i '/ swap / s/^/#/' /etc/fstab
```

### 5. Kernel Modules & Sysctl

```bash
sudo modprobe overlay br_netfilter
sudo sysctl --system
```

### 6. containerd Configure

```bash
sudo apt install -y containerd
sudo systemctl restart containerd && sudo systemctl enable containerd
```

### 7. Initialize Cluster

```bash
sudo kubeadm init --pod-network-cidr=192.168.0.0/16
```

### 8. kubeconfig Setup

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

### 9. Install Calico

```bash
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```

### 10. Install NGINX Ingress Controller

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.9.1/deploy/static/provider/cloud/deploy.yaml
```

### 11. Create Sample Deployments

```bash
kubectl create deployment frontend --image=nginx
kubectl expose deployment frontend --port=80 --name=frontend-svc
kubectl create deployment backend --image=nginx
kubectl expose deployment backend --port=80 --name=backend-svc
```

### 12. Create & Apply Ingress

`multiapp-ingress.yaml`:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: multiapp-ingress
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: frontend-svc
            port:
              number: 80
      - path: /api
        pathType: Prefix
        backend:
          service:
            name: backend-svc
            port:
              number: 80
```

```bash
kubectl apply -f multiapp-ingress.yaml
kubectl get ingress
```

---

## 📚 Assignment

* Create frontend & backend deployments
* Expose as services
* Use Ingress to route: `/ → frontend`, `/api → backend`


