# RealWorldProject2 — AWS + Terraform + Ansible + Jenkins + Kubernetes CI/CD

## 🏗️ Phase 1 — Core Infrastructure

**Step 0 — Prerequisites**

* AWS Free-tier account + IAM user (Admin)
* Access Key & Secret Key
* Local/EC2 Ubuntu 22.04 ready

**Step 1 — AWS CLI**

```bash
sudo apt update
sudo apt install awscli -y
aws configure
aws s3 ls
```

**Step 2 — Terraform Installation**

```bash
sudo apt install -y unzip curl
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
sudo apt-add-repository "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt update && sudo apt install terraform -y
terraform -v
```

**Step 3 — Terraform Project Setup**

```bash
mkdir -p ~/RealWorldProject2/terraform
cd ~/RealWorldProject2/terraform
touch main.tf variables.tf outputs.tf
terraform init
terraform plan
terraform apply
```

**Step 4 — Ansible Setup**

```bash
sudo apt install ansible -y
ansible-playbook -i inventory.ini webserver.yml
```

**✅ Phase 1 Outcome**

* AWS VPC + Public Subnet + EC2 + SG ready
* Nginx installed via Ansible

---

## 🏗️ Phase 2 — CI/CD + Kubernetes + Monitoring

**Step 1 — Docker on EC2**

```bash
sudo apt install -y docker.io
sudo systemctl enable --now docker
sudo usermod -aG docker ubuntu
docker --version
```

**Step 2 — Kubernetes (Minikube + kubectl)**

```bash
curl -LO minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
minikube start --driver=none

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
kubectl version --client
```

**Step 3 — Jenkins Setup**

```bash
sudo apt install -y openjdk-11-jdk jenkins
sudo systemctl enable --now jenkins
sudo systemctl status jenkins
```

* Access: `http://<JENKINS_PUBLIC_IP>:8080`
* Unlock with initialAdminPassword
* Install recommended plugins + create admin user

**Step 4 — Jenkins Integration**

* Docker & Kubernetes plugin install
* DockerHub credentials configure

**Step 5 — CI/CD Pipeline (Jenkinsfile)**

* Git → Docker build → push → K8s deploy automation

**Step 6 — Kubernetes Deployment Files**

* deployment.yaml, service.yaml, ingress.yaml
* `kubectl apply -f k8s-manifests/`

**Step 7 — Monitoring (Prometheus + Grafana)**

```bash
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install prometheus prometheus-community/prometheus -f monitoring/prometheus-values.yaml
helm repo add grafana https://grafana.github.io/helm-charts
helm install grafana grafana/grafana
```

**Step 8 — Pro Enhancements**

* Private subnets + NAT Gateway
* HTTPS (cert-manager + LetsEncrypt)
* RBAC for Jenkins & K8s
* Horizontal Pod Autoscaler

**✅ Phase 2 Outcome**

* Fully automated CI/CD (Git → Docker → K8s)
* Prometheus + Grafana monitoring
* Production-ready architecture with Nginx deployed

---
