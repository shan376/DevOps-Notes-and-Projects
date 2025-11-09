# Jenkins CI/CD Pipeline for Node.js App

📘 **Jenkinsfile (Declarative Pipeline for Node.js App)**
## Concept
Jenkinsfile aik text file hai jo Jenkins pipeline define karti hai — kaunse steps build, test aur deploy ke liye chalayein jaayen.  
- Declarative syntax: Recommended, readable  
- Scripted syntax: Flexible, Groovy-based  

Project ke root folder mein rakho, Jenkins ye read karke automation run karta hai.  

**Roman Urdu:**  
Jenkinsfile mein likha hota hai ke Jenkins ko kis order mein kaam karna hai. Jaise BookStore Node.js app ho, Jenkins se kehna:  
_"Jab GitHub pe code push ho, ye 4 kaam automatically karo."_  

---

## Practical Implementation

### 1. Install Dependencies
```bash
sudo apt update
sudo apt install openjdk-17-jdk maven nodejs npm -y
java -version
mvn -v
node -v
npm -v
````

### 2. Install & Start Jenkins

```bash
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
sudo systemctl status jenkins
```

Access Jenkins: `http://<your-server-ip>:8080`

---

### 3. Project Setup

```bash
mkdir my-node-app
cd my-node-app
npm init -y
nano Jenkinsfile
```

### 4. Jenkinsfile Example

```groovy
pipeline {
  agent any
  stages {
    stage('Install Dependencies') { steps { sh 'npm install' } }
    stage('Run Tests') { steps { sh 'npm test' } }
    stage('Build') { steps { sh 'npm run build' } }
    stage('Deploy') { steps { echo 'Deploying the application...' } }
  }
}
```

---

### 5. package.json (Dummy Test & Build)

```json
{
  "name": "my-node-app",
  "version": "1.0.0",
  "scripts": {
    "start": "node index.js",
    "test": "echo \"Dummy test passed\" && exit 0",
    "build": "echo \"Build successful\""
  },
  "dependencies": {}
}
```

### 6. pom.xml (Maven support)

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" ...>
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

### 7. GitHub Push

```bash
git init
git remote add origin https://github.com/shan376/node-ci-pipeline.git
git add .
git commit -m "Initial Maven project with Jenkinsfile"
git branch -M main
git push -u origin main
```

### 8. Jenkins Setup

* Add GitHub credentials: Jenkins → Manage Jenkins → Credentials → Global → Username/Personal Access Token
* Create Pipeline Job: Jenkins Dashboard → New Item → Pipeline → Pipeline script from SCM → Git repo URL → Branch */main → Script Path Jenkinsfile
* Webhook setup in GitHub for auto-trigger

---

## Roman Urdu Summary

* Jenkinsfile automation script hai
* Node.js app ke liye 4 stages: install, test, build, deploy
* GitHub push ke baad Jenkins auto-build/test karta hai
* Developer ka kaam easy ho jata hai!

---

✅ **Ready for CI/CD testing**

```

