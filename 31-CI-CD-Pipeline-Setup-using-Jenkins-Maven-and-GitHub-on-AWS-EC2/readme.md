# 🚀  CI/CD Concepts and Practical real world work

This project demonstrates **CI/CD (Continuous Integration & Continuous Deployment)** using **GitHub**, **Maven**, **JUnit**, and **Jenkins** on an **AWS EC2 Ubuntu instance**.

---

## 📘 Concept Overview

| Term | Full Form | Purpose |
|------|------------|----------|
| **CI** | Continuous Integration | Automatically build & test code on every push |
| **CD** | Continuous Delivery / Deployment | Automatically deliver or deploy code to environments |

🧠 **Roman Urdu:**  
- CI: Jab developer GitHub par code push karta hai, system code ko build aur test karta hai.  
- CD: Jab build successful ho jata hai, Jenkins code ko staging/production server par deploy kar deta hai.

---

## 🔁 CI/CD Flow

```

Developer → Git Push → Jenkins (CI Tool)
↓
Run Test Cases
↓
Build Artifact (.jar)
↓
Deploy to Staging/Production

````

---

## ⚙️ Tools Used

- **GitHub** → Code Repository  
- **Maven** → Build Tool  
- **JUnit** → Testing Framework  
- **Jenkins (EC2)** → Automation Engine  

---

## 🧪 Practical Steps (Short Summary)

1. **Launch EC2 Instance (Ubuntu 22.04)**  
   - Use t2.medium  
   - Allow ports: 22 (SSH), 8080 (Jenkins)  

2. **Install Required Tools**
   ```bash
   sudo apt update
   sudo apt install git openjdk-17-jdk maven jenkins -y
````

3. **Start Jenkins**

   ```bash
   sudo systemctl start jenkins
   sudo systemctl enable jenkins
   ```

4. **Access Jenkins**

   ```
   http://<EC2-Public-IP>:8080
   ```

5. **Create Java Project (Maven + JUnit)**

   * `HelloWorld.java` → prints “Hello, DevOps!”
   * `HelloWorldTest.java` → verifies output

6. **Push Project to GitHub**

   ```bash
   git init
   git add .
   git commit -m "Initial Commit"
   git push origin master
   ```

7. **Create Jenkinsfile**

   ```groovy
   pipeline {
     agent any
     stages {
       stage('Build') { steps { sh 'mvn clean compile' } }
       stage('Test') { steps { sh 'mvn test' } }
       stage('Package') { steps { sh 'mvn package' } }
       stage('Deploy') { steps { sh 'cp target/*.jar /home/ubuntu/dev-deploy/' } }
     }
   }
   ```

8. **Run Jenkins Pipeline**
   Jenkins automatically:

   * Pulls code → Builds → Tests → Packages → Deploys

---

## ✅ Final Output

```bash
cd /home/ubuntu/dev-deploy/
java -cp sample-java-app-1.0-SNAPSHOT.jar HelloWorld
```

**Output:**

```
Hello, DevOps!
```

---

## 🎯 Summary

* Complete CI/CD setup with GitHub + Jenkins
* Automated build, test, and deployment
* Real-world DevOps pipeline example
* Roman Urdu explanations included for beginners

---

**Author:** Shan Zafar
**Course:** DevOps Engineering (Topic 31 – CI/CD Concepts)

```
