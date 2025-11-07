# 🧩 Pods aur Deployment in Kubernetes

Kubernetes mein **Pod** sabse chhoti deployable unit hoti hai — ek pod mein ek ya zyada containers ho saktay hain. **Deployment** use hoti hai replicas manage karne ke liye, taake aap easily scale up ya rollback kar sako bina downtime ke.

---

### ⚙️ **Real-Life Example**

Socho aapka web app 3 servers pe chalana hai — to aap deployment se 3 identical pods bana saktay ho. Agar traffic badh jaye to pods ka number badha ke load handle kar saktay ho.

---

### ⚡ **Scale Up Kya Hota Hai**

**Scale Up** ka matlab hai zyada pods banana taake application zyada users handle kar sake.
Example:
1 pod = 100 users
3 pods = 300 users

```bash
kubectl scale deployment webapp --replicas=3
```

➡️ Ye command webapp ke 3 pods bana deti hai.

---

## 🧪 **Practical Work**

### ✅ Step 1: Create Deployment

```bash
kubectl create deployment webapp --image=nginx
```

🧾 Ye command ek nginx deployment banati hai jisme container run hota hai.

---

### ✅ Step 2: Expose Deployment

```bash
kubectl expose deployment webapp --type=NodePort --port=80
```

🧾 Ye command deployment ko NodePort ke zariye expose karti hai taake browser se access ho sake.

---

### ✅ Step 3: Check Service

```bash
kubectl get svc
```

🧾 Ye command batati hai kis NodePort se webapp accessible hai.

---

## 📚 **Assignment Work**

### 🔁 Step 1: Scale the Deployment

```bash
kubectl scale deployment webapp --replicas=3
```

🧾 Is command se 3 pods create ho jate hain — yani scaling up.

---

### 🔁 Step 2: Update Image Version

```bash
kubectl set image deployment/webapp nginx=nginx:1.19
```

🧾 Ye command nginx version update karne ke liye hoti hai.

---

### 🧪 Step 3: Check Deployment Status

```bash
kubectl rollout status deployment/webapp
```

🧾 Ye command batati hai ke deployment ka update successful hua ya nahi.

---

## ✅ **Conclusion**

Is topic mein aapne seekha:

* Pods aur Deployments ka concept
* NodePort se app expose karna
* Deployment scale aur update karna
* Rollout status se deployment monitor karna

**Lab Done Successfully ✅**
