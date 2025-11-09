# 🚀 Jenkins CI/CD Pipeline for Java App Deployment

## 📘 Conceptual Understanding
**English:**  
Jenkins is an open-source automation server used to build, test, and deploy code automatically in a CI/CD pipeline.

**Roman Urdu:**  
Jenkins ek automation tool hai jo code ko automatically build, test aur deploy karta hai.  
Hum GitHub se code lete hain, Jenkins usay Maven se build karta hai aur phir deploy karta hai.

---

## 🧩 Step-by-Step Practical Setup

### ✅ Step 1: Install Java (JDK 17)
```bash
sudo apt update
sudo apt install openjdk-17-jdk -y
java -version
````

### ✅ Step 2: Install Maven

```bash
sudo apt install maven -y
mvn -v
```

### ✅ Step 3: Install Jenkins

```bash
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install jenkins -y
```

### ✅ Step 4: Start & Enable Jenkins

```bash
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

Browser Access → `http://<EC2-Public-IP>:8080`
Unlock Jenkins:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

---

## ⚙️ Install Jenkins Plugins

Dashboard → **Manage Jenkins → Plugins → Available**
Install:

* Git
* GitHub
* Pipeline
* GitHub Branch Source

---

## 🧠 Create Maven Project

```bash
mvn archetype:generate -DgroupId=com.example -DartifactId=ci-demo -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
cd ci-demo
```

### ➕ Add Jenkinsfile

```bash
nano Jenkinsfile
```

Paste:

```groovy
pipeline {
  agent any
  stages {
    stage('Checkout') { steps { checkout scm } }
    stage('Build') { steps { sh 'mvn clean package' } }
    stage('Test') { steps { sh 'mvn test' } }
    stage('Deploy') { steps { echo 'Simulating deployment step' } }
    stage('Notify') { steps { echo 'Build completed successfully!' } }
  }
}
```

---

## 🛠 Fix `pom.xml`

```bash
nano pom.xml
```

Paste:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.shan.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0-SNAPSHOT</version>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.13.2</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```

---

## 🔄 Push to GitHub

```bash
git init
git remote add origin https://github.com/shan376/ci-demo.git
git add .
git commit -m "Initial Maven project with Jenkinsfile"
git branch -M main
git push -u origin main
```

---

## 🔐 Add GitHub Credentials to Jenkins

* Go to: **Manage Jenkins → Credentials → Add**
* Type: *Username with Password*
* Username = GitHub Username
* Password = GitHub Token

---

## 🧩 Create Jenkins Pipeline Job

1. Dashboard → New Item → Name: `ci-demo-pipeline`
2. Type: **Pipeline**
3. Definition: Pipeline script from SCM

   * SCM: Git
   * Repo URL: `https://github.com/shan376/ci-demo.git`
   * Branch: `*/main`
   * Script Path: `Jenkinsfile`

---

## 🔔 Setup GitHub Webhook

GitHub → Repo → **Settings → Webhooks → Add Webhook**

* Payload URL: `http://<jenkins-ip>:8080/github-webhook/`
* Content type: `application/json`
* Event: **Just the push event**

In Jenkins → **Build Triggers → Enable**
☑️ GitHub hook trigger for GITScm polling

---

## 🧪 Final Testing

1. Edit file in GitHub repo
2. Push changes
3. Jenkins auto-trigger karega and build run hoga ✅

---

## 📊 Roman Urdu Summary

* EC2 Ubuntu instance launch kiya
* Java, Maven, Jenkins install aur configure kiya
* Jenkinsfile likhi aur GitHub se connect kiya
* Webhook set kiya taake push hone par auto build ho
* Jenkins ne automatically code build, test aur deploy kiya 🎯

---

## 🧾 Real-Life Example (Optional)

BookBazaar app jaisi Java project ke liye yehi CI/CD pipeline use hoti hai.
Developer code push karta hai → Jenkins build/test/deploy karta hai automatically 🚀

---

### ✅ Final Result:

| Component         | Status |
| ----------------- | ------ |
| Java Installed    | ✅      |
| Maven Installed   | ✅      |
| Jenkins Installed | ✅      |
| GitHub Connected  | ✅      |
| Webhook Triggered | ✅      |
| Build Successful  | ✅      |

---

**💬 Roman Urdu Recap:**
"Ye setup ek complete CI/CD automation pipeline hai — GitHub se code push hote hi Jenkins automatically build, test aur deploy karta hai. Yehi real-world DevOps ka workflow hai!"

```
