# 🧩 Kubernetes Services & YAML Configuration

Is topic mein humne **Service** aur **YAML configuration** ke bare mein seekha.
Service ek fixed IP provide karti hai taake pod reliable rahe even agar restart ho jaye.
YAML file se hum Kubernetes resources (jaise Pod, Service) ko define karte hain — simple aur human-readable format mein.

---

### ⚙️ Prerequisites

* Kubernetes cluster running ho (Minikube, Kind, ya Cloud).
* `kubectl` properly configured ho.
* YAML files ke liye working directory set ho.

---

### 🧪 Practical Work

#### ✅ Step 1: Pod YAML Banana

**Command:**

```bash
nano pod.yaml
```

**YAML File Content:**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: demo-pod
  labels:
    app: demo
spec:
  containers:
  - name: nginx-container
    image: nginx
```

🧾 *Roman Urdu:* Ye file ek pod define karti hai jisme nginx container chalega.

**Apply Command:**

```bash
kubectl apply -f pod.yaml
```

🧾 *Roman Urdu:* Ye command pod create kar deti hai.

---

#### ✅ Step 2: Service YAML Banana

**Command:**

```bash
nano service.yaml
```

**YAML File Content:**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: demo-service
spec:
  selector:
    app: demo
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  type: NodePort
```

🧾 *Roman Urdu:* Is YAML se NodePort service banti hai jo pod ko bahar se access karne deti hai.

**Apply Command:**

```bash
kubectl apply -f service.yaml
```

🧾 *Roman Urdu:* Ye command service create kar deti hai.

---

#### ✅ Step 3: Service Verify Karna

**Command:**

```bash
kubectl get svc
```

🧾 *Roman Urdu:* Ye command service list dikhati hai aur NodePort bhi show karti hai.

---

### 🌐 Browser Se Access Karna

Agar aap EC2 use kar rahe hain:

```bash
http://<your-ec2-public-ip>:<NodePort>
```

**Example:**

```bash
http://54.173.50.100:30007
```

⚠️ *Note:* Security Group ya Firewall mein NodePort (30000–32767) open hona chahiye.

---

### ✅ Final Result

* Pod successfully create hua.
* NodePort service ban gayi.
* Browser se Nginx welcome page visible hoga.
