# 🚨 Alertmanager – Alert Setup in Kubernetes from Scratch

## 📘 Conceptual Overview

* **Alertmanager** Prometheus ka companion hai jo alerts manage karta hai: email, Slack, webhook bhejna, duplicate alerts group karna, aur silence/inhibit karna.
* **Zarurat:** Automated alerting, centralized management, tool integration, aur repeated alerts ko handle karna.
* **Analogy:** Jaise ek factory mein central system manager ko alert bhejta hai, Alertmanager cluster alerts ko centralize karta hai.

## 🧪 Lab Work

1. Launch EC2 Ubuntu 22.04 Server (t2.medium)
2. Install Docker + Kubernetes + Helm
3. Untaint node for workloads
4. Install Prometheus + Alertmanager + Grafana stack
5. Configure Alertmanager for email
6. Test alert using CPU stress
7. Assignment: Setup working alerts & receive notifications

## 🟢 Practical Steps (Short)

1. **EC2 Setup:** Open ports 22, 3000, 9090, 9093
2. **Docker + K8s:** Install docker.io, containerd, kubeadm, kubelet, kubectl
3. **Init Cluster:** `sudo kubeadm init --pod-network-cidr=192.168.0.0/16`
4. **kubectl Config:** Copy admin.conf to `$HOME/.kube/config`
5. **Install Calico:** `kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml`
6. **Install Helm:** `curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash`
7. **Remove Control Plane Taint:** `kubectl taint nodes --all node-role.kubernetes.io/control-plane-`
8. **Prometheus + Alertmanager via Helm:**

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
kubectl create namespace monitoring
helm install prometheus prometheus-community/kube-prometheus-stack \
--namespace monitoring \
--set alertmanager.persistentVolume.enabled=false \
--set grafana.enabled=true \
--wait --timeout 10m
```

9. **Verify Pods:** `kubectl get pods -n monitoring`
10. **Grafana Access:**

```bash
kubectl port-forward svc/prometheus-grafana -n monitoring 3000:80
kubectl get secret --namespace monitoring prometheus-grafana -o jsonpath="{.data.admin-password}" | base64 -d
```

11. **Custom Alert Rule:** Create `high-cpu-alert-rule.yaml` & apply:

```bash
kubectl apply -f high-cpu-alert-rule.yaml
```

12. **Alertmanager Email Config:** Create `alertmanager.yaml`, delete old secret, create new secret, restart pod:

```bash
kubectl -n monitoring delete secret alertmanager-prometheus-kube-prometheus-alertmanager
kubectl -n monitoring create secret generic alertmanager-prometheus-kube-prometheus-alertmanager --from-file=alertmanager.yaml
kubectl -n monitoring delete pod -l app.kubernetes.io/name=alertmanager
```

13. **Trigger High CPU Alert:**

```bash
sudo apt install stress-ng -y
stress-ng --cpu 4 --timeout 5m
```

14. **Check Prometheus Alerts:** Open Prometheus UI → Alerts tab
15. **Email Received:** FIRING & RESOLVED notifications

## ✅ Assignment

* Configure custom alerts for pods
* Test notifications via email or Slack
* Take screenshots of alerts firing and resolved

---
