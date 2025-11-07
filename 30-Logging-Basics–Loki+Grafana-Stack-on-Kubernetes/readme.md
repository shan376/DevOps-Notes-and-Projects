# Loki + Grafana Logging on Kubernetes (K3s)

## 📘 Concept
Kubernetes cluster mein har pod apne logs generate karta hai, lekin ye scattered hote hain.  
**Centralized Logging** sab logs ek jagah collect karta hai, easy debugging aur monitoring ke liye.  

**Loki:** Grafana Labs ka log aggregator, sirf metadata index karta hai.  
**Grafana:** Logs visualize aur analyze karne ka tool, search aur filter ke features ke saath.  

**Roman Urdu:**  
Loki pods ke logs collect karta hai aur Grafana unhe visualize karta hai. Ye combo centralized logging ke liye best hai.

---

## 🧩 Practical Steps

1. **Pre-Requisites**
   - EC2 Ubuntu (t3.medium), SSH 22, HTTP 80, HTTPS 443, optional NodePort 3000–32767
   - Update Ubuntu & install tools:
   ```bash
   sudo apt update && sudo apt upgrade -y
   sudo apt install curl wget git vim -y
````

* Install Docker:

```bash
curl -fsSL https://get.docker.com | sudo bash
sudo usermod -aG docker $USER
newgrp docker
docker --version
```

* Install K3s:

```bash
curl -sfL https://get.k3s.io | sh -
echo "alias kubectl='k3s kubectl'" >> ~/.bashrc
source ~/.bashrc
```

* Install Helm:

```bash
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
helm version
```

* Create monitoring namespace:

```bash
kubectl create namespace monitoring
kubectl get ns
```

2. **Add Grafana Helm Repo**

```bash
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```

3. **Install Loki Stack**

```bash
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
helm install loki grafana/loki-stack --namespace monitoring
echo 'export KUBECONFIG=/etc/rancher/k3s/k3s.yaml' >> ~/.bashrc
source ~/.bashrc
```

4. **Verify Installation**

```bash
kubectl get all -n monitoring
```

Check pods: loki-0, promtail-xxxxx, grafana-xxxxxx

5. **Access Grafana UI**

```bash
kubectl get svc -n monitoring loki-grafana
kubectl patch svc loki-grafana -n monitoring -p '{"spec": {"type":"NodePort"}}' # if needed
kubectl get svc -n monitoring loki-grafana
```

* Browser: `http://<EC2-Public-IP>:<NodePort>`
* Login: `admin / admin123`

6. **Configure Loki in Grafana**

* Grafana → Settings → Data Sources → Add → Loki
* URL: `http://loki.monitoring.svc.cluster.local:3100` → Save & Test

7. **Import Loki Dashboard**

* Grafana → + → Import → Dashboard ID: 13407 → Loki as Data Source → Import

8. **View Logs**

* Grafana Explore → Loki as source
* Queries: `{app="nginx"}`, `{namespace="default"}`
* Filter by pod, labels, container

---

## 📌 Final Notes

* Loki installed via Helm ✅
* Promtail running ✅
* Loki added in Grafana ✅
* Dashboard imported ✅
* Logs visualized ✅

**Roman Urdu Summary:**
Humne Loki install kiya taake pods ke logs Promtail se collect ho kar Grafana mein visualize hon. Ab easily identify kar sakte hain kaunse pod ne kya log produce kiya aur errors quickly trace ho jate hain.

```
```
