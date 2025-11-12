# 🚀 Prometheus + Grafana — Kubernetes Monitoring from Scratch

## 📘 Overview
Prometheus aur Grafana Kubernetes monitoring ke liye sabse powerful combo hain.  
- **Prometheus**: Automatically metrics collect karta hai (CPU, RAM, Pod Restarts, Network usage).  
- **Grafana**: Un metrics ko visually graphs aur dashboards mein show karta hai.  

## 🧠 Roman Urdu Summary
- Prometheus ek “data collector” hai jo cluster ki har activity record karta hai.  
- Grafana ek “visual screen” hai jo real-time performance, alerts aur errors dikhata hai.  
- Ye dono mil ke DevOps monitoring ka complete solution banate hain.  

## 🛠️ Real-World Need
1. **Real-Time Alerts** – CPU ya memory high hone par Slack/Email alert milta hai.  
2. **Historic Analysis** – 7 din purana trend check karke scaling decision le sakte ho.  
3. **Capacity Planning** – Identify karo kaunsa node over-utilized hai.  
4. **Debugging** – Crash loops aur memory leaks detect karna easy hota hai.  

## 🌍 Used By
| Company | Use Case |
|----------|-----------|
| Netflix | Monitor microservice latency & failures |
| Airbnb  | Load balance & server health check |
| Amazon  | Real-time AWS service dashboards |

## ⚙️ Lab Setup (Ubuntu 22.04)
1. Launch EC2 server → `t2.medium`, open ports `22, 6443, 3000, 9090`.  
2. Install Docker, containerd, Kubernetes & Calico CNI.  
3. Install Helm → `curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash`  
4. Remove taint for single-node cluster:  
   ```bash
   kubectl taint nodes --all node-role.kubernetes.io/control-plane-
````

5. Create namespace:

   ```bash
   kubectl create namespace monitoring
   ```

## 📊 Install Prometheus + Grafana via Helm

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

helm install prometheus-stack prometheus-community/kube-prometheus-stack \
  --namespace monitoring \
  --set prometheus.prometheusSpec.storageSpec.volumeClaimTemplate=null \
  --set alertmanager.alertmanagerSpec.storage.volumeClaimTemplate=null \
  --set grafana.persistence.enabled=false \
  --wait --timeout 10m
```

Verify pods:

```bash
kubectl get pods -n monitoring
```

## 🌐 Access Grafana

```bash
kubectl port-forward svc/prometheus-stack-grafana 3000:80 -n monitoring
```

Browser: `http://localhost:3000`
Login:

```bash
kubectl get secret -n monitoring prometheus-stack-grafana \
  -o jsonpath="{.data.admin-password}" | base64 -d
```

Username: `admin`
Password: (output above)

## 🧩 PuTTY Tunnel (Access from PC)

* In PuTTY → **Connection → SSH → Tunnels**
* Add: `Source port = 3000`, `Destination = localhost:3000`, click **Add**
* Save & Connect → Now open `http://localhost:3000` in your browser ✅

## 🎯 Assignment

1. Open Grafana Dashboard → Explore cluster & pod metrics.
2. Find top 5 pods by memory usage.
3. Bonus: Create your own custom dashboard in Grafana.

## ✅ Final Summary

Prometheus collect karta hai metrics, Grafana unhe visualize karta hai.
Ye setup Kubernetes monitoring, alerting, aur troubleshooting ke liye essential hai.

```

---
