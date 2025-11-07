# 🧩 Kubernetes PV + PVC + NGINX


PV storage space hota hai (almari jaisa) aur PVC us storage ki request (almari ki chabi). Pod PVC ke zariye PV se connect hota hai, taake data delete hone ke baad bhi safe rahe.

---

### ⚙️ Practical Work

**1️⃣ PV Banana:**

```bash
nano pv.yaml
kubectl apply -f pv.yaml
```

🧾 Host machine ke `/mnt/data` folder se storage bind hota hai.

**2️⃣ PVC Banana:**

```bash
nano pvc.yaml
kubectl apply -f pvc.yaml
```

🧾 PVC storage claim karta hai (1Gi).

**3️⃣ Pod Create Karna (PVC Mount ke Sath):**

```bash
nano nginx-pod.yaml
kubectl apply -f nginx-pod.yaml
```

🧾 Pod PVC ko mount karke NGINX run karta hai.

**4️⃣ Host Directory Banana:**

```bash
sudo mkdir -p /mnt/data && sudo chmod 777 /mnt/data
```

**5️⃣ Verify Karna:**

```bash
kubectl get pv,pvc,pods
```

🧾 PV & PVC “Bound”, Pod “Running” hona chahiye.

**6️⃣ Pod ke Andar Data Likho:**

```bash
kubectl exec -it nginx-pod -- /bin/bash
echo "Persistent Storage Test" > /usr/share/nginx/html/index.html
exit
```

**7️⃣ Pod Expose Karna (NodePort):**

```bash
kubectl expose pod nginx-pod --type=NodePort --port=80
kubectl get svc
```

🧾 NodePort copy karo aur browser mein open karo:
`http://<Public-IP>:<NodePort>`

---

### ✅ Final Result

* PV = Bound
* PVC = Bound
* Pod = Running
* Data safe after pod deletion.

---

### 📚 Assignment

PVC = 2Gi banao, new NGINX pod mount karo, expose karo aur browser se check karo.
