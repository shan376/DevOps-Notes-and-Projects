# 🧩 Kubernetes – Docker + K3s + NGINX Deployment

Kubernetes (K8s) ek open-source system hai jo multiple Docker containers ko manage karta hai — crash hone par auto restart karta hai, aur load zyada ho to new containers bana deta hai.

**Example:**
E-commerce app (frontend + backend + database) ko Kubernetes manage karta hai taake sab smooth chale.

---

### ⚙️ **Quick Lab Steps**

**1️⃣ Docker Install**

```bash
sudo apt update && sudo apt install -y docker.io
sudo systemctl enable docker --now
```

➡️ Docker install aur enable karo.

**2️⃣ K3s Install (Lightweight Kubernetes)**

```bash
curl -sfL https://get.k3s.io | sh -
sudo k3s kubectl get nodes
```

➡️ Node ready check karo.

**3️⃣ (Optional) Add SWAP Memory**

```bash
sudo fallocate -l 1G /swapfile && sudo mkswap /swapfile && sudo swapon /swapfile
```

➡️ RAM kam hone pe help karta hai.

**4️⃣ Deploy NGINX**

```bash
sudo k3s kubectl create deployment nginx --image=nginx
sudo k3s kubectl expose deployment nginx --port=80 --type=NodePort
```

➡️ Web server deploy aur expose karo.

**5️⃣ AWS Inbound Rule**
➡️ EC2 > Security Groups > Add Rule
Port Range: **30000–32767**, Source: **Anywhere**

**6️⃣ Access in Browser**

```
http://<EC2-PUBLIC-IP>:<NodePort>
```

➡️ “Welcome to nginx!” dikhe to lab done ✅

---

### ⚠️ Common Error

`ERR_CONNECTION_TIMED_OUT` → Full NodePort range (30000–32767) allow karo.

---

✅ **Result:**
Docker + K3s + NGINX successfully setup ho gaya aur browser se accessible hai.
