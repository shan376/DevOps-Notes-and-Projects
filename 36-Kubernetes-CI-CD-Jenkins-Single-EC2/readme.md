# Kubernetes CI/CD using Jenkins (Single EC2 Instance)

## 📘 Overview 
Ye project dikhata hai ke kaise hum **Jenkins CI/CD** pipelines setup kar sakte hain ek single **EC2 instance** par jo **K3s (Kubernetes)** aur **Docker** ke sath run hota hai.  
- CI/CD ka matlab:  
  - **CI (Continuous Integration):** Code ko frequently test & build karna  
  - **CD (Continuous Deployment/Delivery):** Code changes ko automatically deploy karna  
- Jenkins automatically detect karta hai GitHub code push aur pipelines chalata hai.  
- Kubernetes cluster app ko containerized form mein run karta hai aur auto-deploy workflow enable karta hai.

## 🔧 Tools Used
- Ubuntu 22.04 LTS (EC2 Instance)  
- Docker (for builds & containerization)  
- Jenkins (CI/CD Automation)  
- K3s (Lightweight Kubernetes)

## ⚙️ Practical Steps (Short Description, Roman Urdu)

### Step 1: Launch EC2 Instance
- Ubuntu 22.04 LTS, t2.medium, open ports 22/8080/30000-32767  
- SSH login via terminal/PuTTY  

### Step 2: Initial Configuration
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl wget vim git unzip net-tools gnupg lsb-release software-properties-common apt-transport-https ca-certificates
sudo hostnamectl set-hostname devops-lab-instance
````

### Step 3: Install Docker

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker $USER
newgrp docker
```

### Step 4: Install Java & Jenkins

```bash
sudo apt install openjdk-17-jdk -y
java -version

# Jenkins Repo & Install
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
sudo systemctl status jenkins
```

* Access Jenkins: `http://<EC2-Public-IP>:8080`
* Unlock: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`

### Step 5: Install K3s

```bash
curl -sfL https://get.k3s.io | sh -
sudo k3s kubectl get nodes
```

### Step 6: Configure Jenkins for K3s

* Install plugins: Kubernetes CLI, Pipeline
* Add Kubeconfig as Jenkins secret file (`kubeconfig-id`)

### Step 7: Jenkinsfile for K8s Deployment

```groovy
pipeline {
  agent any
  environment {
    KUBECONFIG = credentials('kubeconfig-id')
  }
  stages {
    stage('Deploy to Kubernetes') {
      steps {
        sh 'kubectl apply -f k8s-deployment.yaml'
      }
    }
  }
}
```

### Step 8: Kubernetes Deployment File (`k8s-deployment.yaml`)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 1
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: nginx
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  type: NodePort
  selector:
    app: myapp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30080
```

### Step 9: Push to GitHub

```bash
git init
git add .
git commit -m "Initial Jenkins + K8s CI/CD pipeline"
git branch -M main
git remote add origin https://github.com/shan376/jenkins-ansible-lab.git
git push -u origin main
```

### Step 10: Assignment Lab (Practice)

1. Launch new EC2 (Ubuntu 22.04)
2. Repeat Docker + Jenkins + K3s setup
3. Clone GitHub repo with Jenkinsfile & k8s-deployment.yaml
4. Create Jenkins pipeline job from GitHub repo
5. Add kubeconfig secret (`kubeconfig-id`)
6. Run pipeline → Check deployment:

```bash
sudo k3s kubectl get pods
sudo k3s kubectl get deployments
sudo k3s kubectl get svc
```

7. Visit App: `http://<EC2-Public-IP>:30080`
   ✅ App should be running via CI/CD pipeline

## 🔗 Result

* Automated deployment of NGINX app on K3s via Jenkins
* Full CI/CD workflow tested on single EC2 instance

```
