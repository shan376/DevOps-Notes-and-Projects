# Real World DevOps Project: Full CI/CD Pipeline

**Tech Stack:** AWS Networking + Kubernetes (kubeadm) + Jenkins CI/CD + Docker + Flask App + Monitoring

---

## Step 1 — AWS Networking

* **VPC:** Create `devops-vpc` CIDR 10.0.0.0/16
* **Subnets:** 3 Public (Bastions), 3 Private (Master, Worker, Jenkins)
* **Internet Gateway & NAT:** Attach IGW to VPC, NAT in public subnet
* **Route Tables:** Public → IGW, Private → NAT
* **Security Groups:** Bastion, Jenkins, Master, Worker, ALB

---

## Step 2 — EC2 Instances

* **Jenkins:** private-subnet-1, t3.medium, Jenkins-SG
* **K8s Master:** private-subnet-2, t3.medium, Master-SG
* **K8s Worker:** private-subnet-3, t3.small, Worker-SG
* **Bastions:** 3x t3.micro, Public subnets, Bastion-SG

---

## Step 3 — Base Setup (All Nodes)

```bash
sudo apt update && sudo apt upgrade -y
sudo swapoff -a
sudo sed -i '/ swap / s/^/#/' /etc/fstab
sudo apt install -y curl apt-transport-https ca-certificates gnupg lsb-release
```

---

## Step 4 — Container Runtime (Master/Worker)

```bash
sudo apt install -y containerd
sudo mkdir -p /etc/containerd
sudo containerd config default | sudo tee /etc/containerd/config.toml
sudo sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' /etc/containerd/config.toml
sudo systemctl restart containerd
sudo systemctl enable containerd
```

---

## Step 5 — Kubernetes (Master/Worker)

```bash
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
sudo kubeadm init --pod-network-cidr=192.168.0.0/16
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
kubectl apply -f https://raw.githubusercontent.com/projectcalico/calico/v3.28.0/manifests/calico.yaml
```

* Workers join using `kubeadm join <MASTER_IP>:6443 --token <TOKEN> --discovery-token-ca-cert-hash sha256:<HASH>`

---

## Step 6 — Jenkins Node Setup

* Install **Java, Jenkins, Docker, kubectl**
* Add `jenkins` user to Docker group
* Configure Jenkins to access K8s via **kubeconfig from S3**

---

## Step 7 — Jenkins K8s Access

* Create ServiceAccount + ClusterRoleBinding
* Generate `jenkins.kubeconfig` → upload to S3
* Jenkins downloads kubeconfig → runs kubectl

---

## Step 8 — Sample App & Jenkinsfile

* **App:** Flask `app.py`, Dockerfile, k8s manifests
* **Pipeline:** Clone → Build → Push Docker → Apply K8s → Rollout Verify
* GitHub repo: `sample-app` → push all files

---

## Step 9 — Jenkins Pipeline Setup

* Jenkins UI → New Item → Pipeline from SCM → GitHub repo
* Add credentials: DockerHub (and GitHub if private)
* Execute → stages run automatically

---

## Step 10 — Deployment Access

* Private cluster → use Bastion SSH Tunnel (NodePort)
* Or ALB → public access
* Prometheus + Grafana → Port-forward / ALB

---

## Step 11 — Rollout & Rollback

```bash
kubectl set image deployment/sample-app sample-app=<image>:v2 --record
kubectl rollout status deployment/sample-app
kubectl rollout undo deployment/sample-app
```

---

## Step 12 — Monitoring

* Helm → install Prometheus + Grafana in `monitoring` ns
* Port-forward: `kubectl port-forward svc/prometheus-grafana -n monitoring 3000:80`
* Login: admin / prom-operator

---

## Step 13 — Best Practices

* Use IAM Roles for EC2 (no static keys)
* Encrypt S3 (SSE), block public access
* Jenkins → least privilege
* Secrets → SSM / Secrets Manager
* Auto-scaling & health checks for workers
* ETCD backup & network ACLs

---

## Step 14 — Troubleshooting Quick Fixes

* Pending Pods: `sudo sysctl -w net.ipv4.ip_forward=1`
* kubeadm join fails: `sudo kubeadm reset -f`, restart kubelet
* Docker permission: add Jenkins to docker group, restart

---

