# Jenkins + Ansible Integration (Complete Notes)

## Conceptual Understanding (Roman Urdu)
Ansible ek configuration management tool hai jo servers configure karta hai aur apps ko deploy karne mein help karta hai.  
Jenkins ek automation server hai jo jab bhi koi code GitHub pe push ho, to automatically tasks run kar sakta hai (jaise build, test, deploy).  

**Integration ka faida:**  
Jenkins code push detect karta hai → Ansible playbook auto-run hoti hai → app deploy ho jata hai bina manual kaam ke.  

**Zarurat Kyun Hai?**  
- Manual deployment har dafa time-consuming aur risky hoti hai.  
- Jenkins + Ansible se automation aata hai.  
- Har code change par app automatic deploy ho jati hai = 💯 CI/CD workflow  

**Real-Life Example:**  
Developer GitHub main branch me naye features push karta hai → Jenkins detect karta hai → Ansible playbook run hoti hai → app update ho jati hai bina manual intervention ke.

---

## Practical Lab Steps

### Step 1: Launch Ubuntu EC2 Instance
- Ubuntu 22.04, t2.medium  
- Open port 8080 for Jenkins  
- SSH login via terminal or PuTTY  

### Step 2: Install Java & Jenkins
```bash
sudo apt update
sudo apt install openjdk-17-jdk -y
java -version

wget -q -O - https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
````

### Step 3: Access Jenkins Web UI

* URL: `http://<your-ec2-public-ip>:8080`
* Unlock Jenkins: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
* Install suggested plugins & create admin user

### Step 4: Install Ansible

```bash
sudo apt update
sudo apt install ansible -y
```

### Step 5: Allow Jenkins to Run Ansible

```bash
sudo usermod -aG sudo jenkins
sudo visudo
# Add at bottom:
jenkins ALL=(ALL) NOPASSWD: ALL
```

### Step 6: Create Jenkinsfile & Ansible Playbook

* Folder: `jenkins-ansible-lab`
* `deploy.yml` (Ansible Playbook):

```yaml
- hosts: localhost
  tasks:
    - name: Create a file to simulate app deployment
      file:
        path: /tmp/app-deployed.txt
        state: touch
```

* `Jenkinsfile`:

```groovy
pipeline {
  agent any
  environment {
    ANSIBLE_HOST_KEY_CHECKING = 'False'
  }
  stages {
    stage('Run Ansible') {
      steps {
        sh 'ansible-playbook deploy.yml'
      }
    }
  }
}
```

### Step 7: Push Project to GitHub

```bash
git init
git add .
git commit -m "Initial Jenkins + Ansible pipeline"
git branch -M main
git remote add origin https://github.com/shan376/jenkins-ansible-lab.git
git push -u origin main
```

### Step 8: Add GitHub Credentials to Jenkins

* Jenkins Dashboard → Manage Jenkins → Credentials → Add Credentials
* Kind: Username with Password → Use GitHub PAT → ID: github-creds

### Step 9: Create Jenkins Pipeline Job

* Jenkins Dashboard → New Item → ansible-deploy-pipeline → Pipeline
* SCM: Git → Repo URL → Credentials: github-creds → Branch: */main → Save

### Step 10: Run Jenkins Job (Manual)

* Build Now → Check Console → Verify `/tmp/app-deployed.txt` exists

### Step 11: Setup GitHub Webhook (Auto Trigger)

* GitHub → Settings → Webhooks → Payload URL: `http://<jenkins-ec2-ip>:8080/github-webhook/`
* Trigger: Just push event → Save
* Jenkins Job → Configure → Tick “GitHub hook trigger for GITScm polling”

### Step 12: Final Test

* Modify code → Push to GitHub
* Jenkins auto-triggers → Ansible runs → `/tmp/app-deployed.txt` updated
* Visit App: `http://<target-ec2-ip>:3000` → Output: `Hello from Jenkins + Ansible + Docker!`

---

## GitHub Repo Structure

```
myrepo35/
├── inventory.ini
├── playbook.yml
├── Jenkinsfile
├── Dockerfile
├── package.json
├── index.js
```

### inventory.ini

```ini
[web]
<target-ec2-public-ip> ansible_user=ubuntu ansible_ssh_private_key_file=/var/lib/jenkins/.ssh/deployement-server-key.pem
```

### playbook.yml

```yaml
- name: Deploy App using Docker
  hosts: web
  become: true
  tasks:
    - name: Install Docker
      apt:
        name: docker.io
        state: present
        update_cache: true
    - name: Clone App Repo
      git:
        repo: https://github.com/shan376/repo1.git
        dest: /home/ubuntu/app
        force: yes
    - name: Build Docker Image
      command: docker build -t myapp:latest .
      args:
        chdir: /home/ubuntu/app
    - name: Stop Running Container (if any)
      command: docker stop myapp
      ignore_errors: yes
    - name: Run Docker Container
      command: docker run -d -p 3000:3000 --name myapp myapp:latest
```

### Jenkinsfile

```groovy
pipeline {
    agent any
    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'
        SSH_DIR = "${env.WORKSPACE}/.ssh"
    }
    stages {
        stage('Clone Repo') {
            steps {
                checkout([$class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/shan376/repo.git',
                        credentialsId: 'put your credential id'
                    ]]
                ])
            }
        }
        stage('Run Ansible') {
            steps {
                withCredentials([file(credentialsId: 'ansible_key', variable: 'PEM_FILE')]) {
                    sh '''
                        mkdir -p $SSH_DIR
                        chmod 700 $SSH_DIR
                        cp $PEM_FILE $SSH_DIR/app-server.pem
                        chmod 600 $SSH_DIR/app-server.pem
                        ssh-keyscan -H deployement-server-ip >> $SSH_DIR/known_hosts
                        ansible-playbook -i inventory.ini playbook.yml --private-key=$SSH_DIR/app-server.pem
                    '''
                }
            }
        }
    }
}
```

### Dockerfile

```dockerfile
FROM node:16
WORKDIR /app
COPY . .
RUN npm install
CMD ["npm", "start"]
```

### package.json

```json
{
  "name": "myapp",
  "version": "1.0.0",
  "description": "Simple Node App",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "test": "echo \"Test passed\" && exit 0",
    "build": "echo \"Build successful\""
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

### index.js

```javascript
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send("Hello from Jenkins + Ansible + Docker!");
});

app.listen(3000, () => {
  console.log('App running on port 3000');
});
```

---

## Push to GitHub

```bash
git init
git remote add origin https://github.com/shan376/repo1.git
git add .
git commit -m "all files for commit"
git branch -M main
git push -u origin main
```

## Jenkins Pipeline Configuration

* SCM: Git → URL: [https://github.com/shan376/repo1.git](https://github.com/shan376/repo1.git) → Credentials: GitHub PAT → Branch: */main
* Use Jenkins Credentials (Secret File) for `.pem` → ID: ansible_key
* Trigger: GitHub hook trigger for GITScm polling

---

✅ **Final Test:** Modify code → Push → Jenkins triggers → Ansible deploy → Visit App → Output: `Hello from Jenkins + Ansible + Docker!`

```

