# Docker in Jenkins (From Scratch)

## Practical Steps & Assignments

1. **Launch EC2 Instance (Ubuntu 22.04 LTS)**
- AWS Console → EC2 → Launch Instance
- Instance type: t2.micro (Free tier)
- Key pair select karo
- Security Group: Allow 22 (SSH), 8080 (Jenkins), 3000 (App)

2. **Connect EC2 via SSH**
```bash
ssh -i "your-key.pem" ubuntu@your-public-ip
````

* Roman Urdu: EC2 server se connect karo terminal ke zariye

3. **Install Java**

```bash
sudo apt update
sudo apt install openjdk-17-jdk -y
java -version
```

* Roman Urdu: Jenkins ke liye Java install karo

4. **Install Jenkins**

```bash
wget -q -O - https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

* Access Jenkins: [http://your-public-ip:8080](http://your-public-ip:8080)
* Initial Admin Password: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`

5. **Install Docker**

```bash
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
```

* Roman Urdu: Docker install aur start karo

6. **Add Jenkins to Docker Group**

```bash
sudo usermod -aG docker jenkins
sudo systemctl restart jenkins
```

* Roman Urdu: Jenkins user Docker command run kar sake

7. **Create Node.js Project**

```bash
mkdir myapp && cd myapp
npm init -y
nano index.js
```

* Sample code:

```javascript
const http = require('http');
http.createServer((req, res) => {
  res.end('Hello from Dockerized Node.js App!');
}).listen(3000);
```

8. **Create Multi-Stage Dockerfile**

```bash
nano Dockerfile
```

* Paste:

```dockerfile
# Stage 1 - Build
FROM node:16 as builder
WORKDIR /app
COPY . .
RUN npm install
RUN npm run build || echo "No build script"

# Stage 2 - Final
FROM node:16
WORKDIR /app
COPY --from=builder /app .
EXPOSE 3000
CMD ["node", "index.js"]
```

9. **Create Jenkinsfile for Docker Pipeline**

```bash
nano Jenkinsfile
```

* Paste:

```groovy
pipeline {
  agent any
  stages {
    stage('Build Docker Image') {
      steps { sh 'docker build -t myapp:latest .' }
    }
    stage('Run Container') {
      steps { sh 'docker run -d -p 3000:3000 myapp:latest' }
    }
  }
}
```

10. **Setup package.json**

```bash
nano package.json
```

* Paste:

```json
{
  "name": "my-node-app",
  "version": "1.0.0",
  "description": "Simple Node.js app for Jenkins CI/CD testing",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "test": "echo \"Dummy test passed\" && exit 0",
    "build": "echo \"Build successful\""
  },
  "author": "Shan",
  "license": "ISC",
  "dependencies": {}
}
```

11. **Push to GitHub**

```bash
git init
git remote add origin https://github.com/yourusername/docker-in-jenkins.git
git add .
git commit -m "Initial commit with Docker and Jenkinsfile"
git branch -M main
git push -u origin main
```

12. **Configure Jenkins Pipeline Job**

* Jenkins Dashboard → New Item → docker-node-pipeline → Pipeline → OK
* Pipeline script from SCM → Git → Repo URL → Branch */main → Script path: Jenkinsfile

13. **Enable GitHub Trigger**

* Jenkins Job → Configure → Build Triggers → Tick: GitHub hook trigger for GITScm polling

14. **Setup Webhook in GitHub**

* GitHub Repo → Settings → Webhooks → Add Webhook
* Payload URL: http://<jenkins-ip>:8080/github-webhook/
* Content type: application/json, Event: push, Active → Save

15. **Debug Docker Port Issues**

```bash
docker ps        # Check running containers
docker stop <id> # Stop container using port 3000
docker ps        # Confirm stopped
```

* Rerun Jenkins job

16. **Test the Pipeline**

* Edit & push any file → Jenkins auto-run build & deploy
* Visit: http://<your-ec2-ip>:3000 → See message

17. **Bonus Docker Commands**

```bash
docker ps
docker stop <id>
docker rm <id>
docker rmi myapp:latest
```

## Roman Urdu Summary

* EC2 launch kiya (Ubuntu)
* Jenkins install kiya
* Docker install aur Jenkins ko Docker group mein dala
* Node.js app banayi
* Multi-stage Dockerfile aur Jenkinsfile likha
* GitHub se Jenkins pipeline connect ki
* App Docker ke zariye deploy ho gayi CI/CD se!

```

