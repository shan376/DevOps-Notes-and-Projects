# Kubernetes Production Best Practices & Setup

### Key Concepts
1. **Security**
- Use RBAC to limit user permissions.
- Store sensitive data in Secrets, not ConfigMaps.
- Apply Network Policies for pod-to-pod communication control.

2. **Scaling**
- Use Horizontal Pod Autoscaler (HPA) for CPU/memory-based scaling.
- Cluster Autoscaler adds/removes nodes automatically.

3. **Monitoring**
- Use Metrics Server for basic metrics.
- Integrate Prometheus + Grafana for dashboards & alerts.

4. **Configuration Management**
- Use ConfigMaps & Secrets for environment configs.
- Helm/Kustomize for deployment management.

5. **High Availability & Fault Tolerance**
- Run multiple replicas across nodes.
- Use Liveness & Readiness probes for automatic restarts.

---

## Practical Objective
- Deploy a production-ready Kubernetes cluster.
- Implement security, scaling, and monitoring.
- Deploy a scalable sample app (Nginx) as assignment.

---

## Base System Setup (All Nodes)
```bash
sudo apt update && sudo apt upgrade -y
sudo swapoff -a
sudo sed -i '/ swap / s/^/#/' /etc/fstab
# Enable modules
echo -e "overlay\nbr_netfilter" | sudo tee /etc/modules-load.d/k8s.conf
sudo modprobe overlay
sudo modprobe br_netfilter
# Sysctl
echo -e "net.bridge.bridge-nf-call-iptables=1\nnet.bridge.bridge-nf-call-ip6tables=1\nnet.ipv4.ip_forward=1" | sudo tee /etc/sysctl.d/k8s.conf
sudo sysctl --system
````

## Container Runtime

```bash
sudo apt install -y containerd
sudo mkdir -p /etc/containerd
containerd config default | sudo tee /etc/containerd/config.toml
sudo sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' /etc/containerd/config.toml
sudo systemctl restart containerd
sudo systemctl enable containerd
```

## Kubernetes Installation

```bash
# Add repo & key
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/deb/ /" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt update
sudo apt install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

## Cluster Setup

* **Master Node**: `sudo kubeadm init --pod-network-cidr=10.244.0.0/16`
* Configure kubectl:

```bash
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
kubectl get nodes
```

* Install network plugin (Flannel):

```bash
kubectl apply -f https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml
```

* **Worker Nodes**: Join via kubeadm join command from master.

---

## Security & Policies

* Create admin service account & RBAC:

```bash
kubectl create serviceaccount admin-user -n kube-system
kubectl create clusterrolebinding admin-user-binding --clusterrole=cluster-admin --serviceaccount=kube-system:admin-user
```

* Example NetworkPolicy (frontend → backend):

```bash
kubectl apply -f network-policy.yaml
```

## Scaling Example

* Deploy Nginx:

```bash
kubectl apply -f nginx-deployment.yaml
kubectl scale deployment nginx-deployment --replicas=5
kubectl autoscale deployment nginx-deployment --cpu-percent=50 --min=2 --max=10
```

## Monitoring

* Metrics Server: `kubectl apply -f components.yaml`
* Prometheus + Grafana via Helm:

```bash
kubectl create namespace monitoring
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install prometheus prometheus-community/kube-prometheus-stack -n monitoring
```

* Access via port-forward / SSH tunnel:

  * Grafana: localhost:3000
  * Prometheus: localhost:9090

---

## Summary

* Production-ready Kubernetes cluster built from scratch.
* Security (RBAC, Network Policies), monitoring (Metrics Server, Prometheus, Grafana), and scaling (HPA) implemented.
* Sample scalable app deployed (Nginx) with proper production architecture.

```
