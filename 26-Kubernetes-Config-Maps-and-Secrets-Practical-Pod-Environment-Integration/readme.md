# 🧩 ConfigMaps & Secrets in Kubernetes

ConfigMap non-sensitive data store karta hai (jaise `APP_MODE`), jabke Secret sensitive info (jaise `password`, `API key`) ko securely rakhta hai.

---

### ⚙️ Practical Work

**1️⃣ ConfigMap Banana:**

```bash
kubectl create configmap app-config --from-literal=APP_MODE=production
```

🧾 Non-sensitive config store hui.

**2️⃣ Secret Banana:**

```bash
kubectl create secret generic db-secret --from-literal=DB_PASSWORD=Pass123
```

🧾 Password securely store hua.

**3️⃣ Pod Mein Attach Karna:**

```yaml
env:
- name: APP_MODE
  valueFrom:
    configMapKeyRef:
      name: app-config
      key: APP_MODE
- name: DB_PASSWORD
  valueFrom:
    secretKeyRef:
      name: db-secret
      key: DB_PASSWORD
```

🧾 ConfigMap aur Secret dono pod ke env variables se attach hue.

---

### ✅ Result

* `APP_MODE=production` (ConfigMap se)
* `DB_PASSWORD=Pass123` (Secret se)

App secure aur easily configurable ban gayi.
