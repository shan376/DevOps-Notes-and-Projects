# 🧩 **Jenkins in DevOps**

## 📘 **Overview**

Jenkins is an open-source automation server used in DevOps for **Continuous Integration (CI)** and **Continuous Deployment (CD)**.
It automates the process of building, testing, and deploying software with minimal manual effort.

### 🔹 **Key Features**

* Automates repetitive DevOps tasks
* Supports plugins (Git, Docker, Kubernetes, etc.)
* Web-based dashboard
* Integrates with GitHub and other SCM tools
* Build triggers (manual, webhook, or schedule)

---

## ⚙️ **Jenkins Architecture**

Jenkins follows a **Master-Agent** (Controller-Agent) architecture:

* **Master/Controller:** Handles scheduling, monitoring, and reporting.
* **Agent/Node:** Executes jobs on the same or remote systems.

---

## 🖥️ **Installation (Ubuntu/Linux)**

**Step-by-Step:**

1. Update system

   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
2. Install Java

   ```bash
   sudo apt install openjdk-11-jdk -y
   ```
3. Add Jenkins repository

   ```bash
   wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
   sudo sh -c 'echo deb https://pkg.jenkins.io/debian binary/ > /etc/apt/sources.list.d/jenkins.list'
   ```
4. Install and start Jenkins

   ```bash
   sudo apt update && sudo apt install jenkins -y
   sudo systemctl start jenkins
   sudo systemctl enable jenkins
   ```
5. Access Jenkins:
   **[http://your-server-ip:8080](http://your-server-ip:8080)**

---

## 🔐 **First-Time Setup**

1. Get admin password:

   ```bash
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```
2. Paste it into Jenkins web UI
3. Install suggested plugins
4. Create admin user
5. Explore Dashboard → *New Item*, *Build History*, *Manage Jenkins*

---

## 🚀 **Create First Jenkins Job**

**Steps:**

1. New Item → Freestyle Project → OK
2. Add “Execute Shell” in the build section
3. Command:

   ```bash
   echo "Hello from Jenkins Job"
   ```
4. Save → Build Now → View console output

✅ You’ve successfully created your first Jenkins job!

---

# 📘 **Jenkins Advanced Topics**

## 🔗 **GitHub Integration**

**Steps:**

1. Install Git plugin (Manage Plugins → Git)
2. Create **Personal Access Token** (PAT) on GitHub
3. Add credentials in Jenkins → Manage Jenkins → Credentials → Add (GitHub PAT)
4. Configure Freestyle Project:

   * Source Code Management → Git
   * Add repository URL and credentials

---

## ⚡ **Build Triggers**

1. **Manual Trigger:** Click *Build Now*
2. **Poll SCM:** Check GitHub repo for changes

   ```
   H/5 * * * *    # Every 5 minutes
   ```
3. **Webhook Trigger (Recommended):**

   * Add GitHub Webhook →
     Payload URL: `http://<jenkins-ip>:8080/github-webhook/`
   * Event: *Push*

✅ Jenkins will build automatically when code is pushed to GitHub.

---

## ⏰ **Job Scheduling (Cron)**

| Schedule Type | Cron Syntax    |
| ------------- | -------------- |
| Every 15 mins | `H/15 * * * *` |
| Daily at 2 AM | `0 2 * * *`    |
| Every Monday  | `0 9 * * 1`    |

---

## 🧠 **Assignment Task**

🎯 **Objective:**
Link Jenkins with GitHub and create an automated job that triggers on each commit.

**Steps Summary:**

1. Install Git plugin
2. Add GitHub credentials (PAT)
3. Create a Freestyle job
4. Enable GitHub webhook trigger
5. Setup webhook in GitHub repo
6. Push new code → Jenkins builds automatically

---

## ✅ **Quick Summary**

* Jenkins automates CI/CD pipelines.
* Jobs define build/test/deploy steps.
* GitHub integration enables auto builds on commits.
* Triggers control when builds happen (manual, webhook, or cron).
* Dashboard helps manage jobs, plugins, and build history.

