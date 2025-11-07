# Prometheus + Grafana Monitoring Lab on K3s

**Objective:**
Deploy **Prometheus** and **Grafana** on a K3s Kubernetes cluster using Helm for real-time monitoring and visualization.

---

## 🧠 Overview (Roman Urdu)

* **Prometheus:** Metrics collector (CPU, memory, pod health, HTTP requests) jo time-series DB mein store karta hai. Active monitoring karta hai.
* **Grafana:** Dashboard tool jo Prometheus se data read karke beautiful graphs aur alerts banata hai.

**Scenario:** E-commerce site mein services monitor karni hain. Prometheus data collect karega aur Grafana se visualize aur alert milega.

---

## 🔧 Lab Prerequisites

| Requirement      | Status                          |
| ---------------- | ------------------------------- |
| EC2 Instance     | ✅ t3.small+ with 20+ GB storage |
| Docker Installed | ✅                               |
| K3s Installed    | ✅                               |
| Helm Installed   | ✅                               |
| kubectl Working  | ✅                               |

---

## ⚡ Steps (Roman Urdu + Commands)

1. **EC2 Setup & Storage**

```bash
sudo mkfs -t ext4 /dev/xvdf
sudo mkdir /mnt/data
sudo mount /dev/xvdf /mnt/data
echo '/dev/xvdf /mnt/data ext4 defaults,nofail 0 0' | sudo tee -a /etc/fstab
```

2. **Docker**

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker $USER
```

3. **K3s Installation**

```bash
curl -sfL https://get.k3s.io | sh -
sudo k3s kubectl get nodes
```

4. **Helm Repos & Namespace**

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
kubectl create namespace monitoring
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
```

5. **Install Prometheus**

```bash
helm install prometheus prometheus-community/prometheus --namespace monitoring
kubectl patch svc prometheus-server -n monitoring -p '{"spec": {"type": "NodePort"}}'
kubectl get svc -n monitoring prometheus-server
```

* **Access:** `http://<EC2-Public-IP>:<NodePort>`

6. **Install Grafana**

```bash
helm install grafana grafana/grafana --namespace monitoring --set adminPassword='admin123' --set service.type=NodePort
kubectl get svc -n monitoring grafana
```

* **Access:** `http://<EC2-Public-IP>:<NodePort>`
* **Login:** `admin/admin123`

7. **Configure Grafana**

* Add Prometheus as Data Source: `http://prometheus-server.monitoring.svc.cluster.local`
* Import Dashboard ID: `1860`

---

## ✅ Lab Summary

* EC2 Launched & Storage Added ✅
* Docker & K3s Installed ✅
* Helm Installed ✅
* Prometheus + Grafana Deployed & Dashboard Configured ✅


