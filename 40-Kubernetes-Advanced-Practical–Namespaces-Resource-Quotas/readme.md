# Kubernetes Advanced Topics – Namespaces & Resource Quotas

## 📘 Goal

Fully understand and implement Kubernetes namespaces and resource quotas.
You will:

1. Create isolated namespaces (`dev` & `qa`).
2. Apply resource quotas on `dev`.
3. Deploy pods respecting resource limits.

---

## 🧠 Concept

* **Namespace:** Logical partition in a cluster to isolate teams/environments.
* **ResourceQuota:** Limits resources like CPU, memory, and pods per namespace.

**Roman Urdu:**
Namespaces se multiple teams ek hi cluster pe conflict-free kaam kar saktay hain.
ResourceQuota se fair resource usage aur cluster stability ensure hoti hai.

**Example:** Dev, QA, Ops teams ek cluster use kar rahe hain → alag namespaces aur quotas se stability aur security milti hai.

---

## ✅ Required Environment

* **EC2 OS:** Ubuntu 22.04 LTS
* **Instance Type:** t2.medium+
* **Ports:** 22, 6443, 8285/UDP, 8472/UDP, 30000–32767 (NodePort optional)

---

## 🚀 Installation & Setup Steps

### 1. Update & Install Docker

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y docker.io
sudo systemctl enable docker --now
```

### 2. Add Kubernetes Repo & Install Tools

```bash
sudo apt install -y apt-transport-https ca-certificates curl gpg
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key | gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /" | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt update
sudo apt install -y kubelet kubeadm kubectl cri-tools
sudo apt-mark hold kubelet kubeadm kubectl
```

### 3. Enable Kernel Modules

```bash
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF
sudo modprobe br_netfilter

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF
sudo sysctl --system
```

### 4. Disable Swap

```bash
sudo swapoff -a
sudo sed -i '/ swap / s/^/#/' /etc/fstab
```

### 5. Optional /etc/hosts Fix

```bash
echo "127.0.0.1 localhost" | sudo tee /etc/hosts
echo "$(curl -s ifconfig.me) master-node" | sudo tee -a /etc/hosts
```

### 6. Initialize Kubernetes Cluster

```bash
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
```

⚠️ If warnings, add `--ignore-preflight-errors=FileContent--proc-sys-net-bridge-bridge-nf-call-iptables`

### 7. Setup kubectl for Current User

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

### 8. Install Flannel CNI

```bash
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

### 9. Verify Node & Flannel

```bash
kubectl get nodes
kubectl get pods -n kube-flannel
```

---

## 🧪 Namespace & Resource Quota

### Create Namespaces

```bash
kubectl create namespace dev
kubectl create namespace qa
```

### Apply Resource Quota

```yaml
# dev-quota.yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: dev-quota
  namespace: dev
spec:
  hard:
    pods: "5"
    requests.cpu: "1"
    requests.memory: "1Gi"
    limits.cpu: "2"
    limits.memory: "2Gi"
```

Apply:

```bash
kubectl apply -f dev-quota.yaml
kubectl describe resourcequota dev-quota -n dev
```

### Deploy Pod in `dev`

```bash
kubectl run nginx --image=nginx -n dev
kubectl get pods -n dev
```

### Test Quota Limit

```bash
kubectl run nginx2 --image=nginx -n dev
kubectl run nginx3 --image=nginx -n dev
kubectl run nginx4 --image=nginx -n dev
kubectl run nginx5 --image=nginx -n dev
kubectl run nginx6 --image=nginx -n dev # ❌ Should fail
```

---

## ✅ Reboot-Proof Fix

```bash
sudo systemctl enable containerd
sudo systemctl enable kubelet
```

---

## ✅ Troubleshooting Essentials

```bash
sudo crictl ps -a | grep kube-apiserver
sudo crictl logs <container_id>
kubectl get pods -n kube-system
kubectl describe pod -n kube-system coredns-xxxx
sudo systemctl daemon-reexec
sudo systemctl restart containerd kubelet
```

---

## ✅ Summary

* Namespaces isolate teams/environments.
* ResourceQuota ensures fair resource usage & stability.
* Max pods, CPU, memory per namespace can be defined.
* Real-world use in DevOps teams, production clusters, and cost-saving scenarios.

---

**Folder Structure:**

```
N/A – Kubernetes cluster on EC2, no local files required
```

