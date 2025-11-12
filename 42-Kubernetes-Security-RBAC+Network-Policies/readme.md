# Kubernetes Security (RBAC + Network Policies)

## 📘 Overview
This lab covers **Role-Based Access Control (RBAC)** and **Network Policies** to secure Kubernetes clusters.  
- **RBAC** defines what users or services can perform within the cluster.  
- **Network Policies** control pod-to-pod communication using ingress/egress rules.

## 🧠 Roman Urdu Summary
- RBAC decide karta hai ke kis user ko kya permission milti hai (e.g., sirf pods dekhna).  
- Network Policy ek firewall jaisa hota hai jo pods ke darmiyan communication control karta hai.  

## 🔁 Real-Life Use Cases
- QA team sirf logs dekh sakti hai, deploy nahi (via RBAC).  
- Backend sirf frontend se baat kare, kisi aur pod se nahi (via NetworkPolicy).  

## 🧪 Lab Setup (Ubuntu 22.04)
1. Launch EC2 instance (2 vCPU, 4GB RAM).  
2. Open ports: `22`, `6443`, `4789` (Calico).  
3. Install Docker, Kubernetes, and Calico CNI.  
4. Disable swap, configure containerd, and initialize cluster.  
5. Verify setup:
   ```bash
   kubectl get nodes
   kubectl get pods -A
````

## 🔐 RBAC Implementation

1. Create namespace:

   ```bash
   kubectl create namespace dev
   ```
2. Create `role.yaml` (read-only access):

   ```yaml
   kind: Role
   apiVersion: rbac.authorization.k8s.io/v1
   metadata:
     namespace: dev
     name: pod-reader
   rules:
   - apiGroups: [""]
     resources: ["pods"]
     verbs: ["get", "watch", "list"]
   ```
3. Bind role to user:

   ```bash
   kubectl apply -f rolebinding.yaml
   ```

## 🌐 Network Policy Implementation

1. Create frontend & backend pods:

   ```bash
   kubectl create deployment frontend --image=nginx -n dev
   kubectl create deployment backend --image=nginx -n dev
   ```
2. Apply `network-policy.yaml`:

   ```yaml
   kind: NetworkPolicy
   apiVersion: networking.k8s.io/v1
   metadata:
     name: allow-frontend
     namespace: dev
   spec:
     podSelector:
       matchLabels:
         app: backend
     ingress:
     - from:
       - podSelector:
           matchLabels:
             app: frontend
   ```

## ✅ Verification

```bash
kubectl get pods -n dev
kubectl get role,rolebinding -n dev
kubectl describe networkpolicy allow-frontend -n dev
```

## 🧩 Assignment

* Create namespace: `secure-zone`
* RBAC: only list pods allowed
* NetworkPolicy: allow traffic only `frontend → backend`

## 🏁 Summary

✔️ RBAC implemented (read-only access)
✔️ NetworkPolicy applied (restricted communication)
✔️ Kubernetes cluster secured from unauthorized access

```

---
