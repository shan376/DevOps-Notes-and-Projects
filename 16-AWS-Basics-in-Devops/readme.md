# 🧠 Topic 16: AWS Basics – Understanding + Ubuntu Lab Guide

## 📘 What is AWS?

AWS (Amazon Web Services) is a cloud platform that provides online servers, storage, databases, and other IT services without needing any physical hardware.
**Roman Urdu:** AWS aik cloud company hai jisme aap server rent par le kar website, software, ya application chala saktay hain.

### 🔍 Real-Life Example:

For an online clothing store:

* EC2 → for website server
* S3 → to store product images & videos
* RDS → to manage payment database

👉 Your full business runs on the cloud — no physical server needed.

---

## 🔧 Practical Work: Create AWS Free Tier Account

1. Go to [https://aws.amazon.com/free](https://aws.amazon.com/free)
2. Click **Create Free Account**
3. Enter Email, Password, Account name, Billing info (card required)
4. Verify phone & email → Choose Free Tier plan → Login to AWS Console

---

## 📑 Assignment: Launch EC2 Instance (Ubuntu)

### 🖥 Goal:

Launch an EC2 instance and connect via SSH (Ubuntu or PuTTY).

#### Steps:

1. **Go to EC2 Dashboard** → Launch Instance
2. **Select AMI:** Ubuntu Server 22.04 LTS
3. **Instance Type:** t2.micro (Free Tier)
4. **Configure Instance:** Name it `MyFirstUbuntuServer`, keep 8 GB storage
5. **Create Key Pair:** `ubuntu-key.pem` (download it)
6. **Set Security Group:**

   * SSH (22) → Anywhere
   * HTTP (80) → Anywhere
7. **Launch Instance**

---

## 🔐 SSH Connection (Using PuTTY on Windows)

1. Convert `.pem` to `.ppk` using PuTTYgen
2. Copy Public IP from EC2 Console
3. In PuTTY → Host: `ubuntu@<your-public-ip>`
4. Go to **SSH → Auth** → Browse `.ppk` file → Connect
5. Login as:

   ```bash
   ubuntu
   sudo su
   ```

---

## 🧪 Lab: Apache Web Server Setup (Inside EC2)

```bash
sudo apt update -y
echo "Hello from EC2 Ubuntu" > index.html
sudo apt install apache2 -y
sudo mv index.html /var/www/html/
sudo systemctl start apache2
```

### ✅ Test:

Open in browser:
`http://<your-public-ip>`
You should see: **Hello from EC2 Ubuntu**

---

## 🎯 Conclusion:

* Connected to EC2 instance
* Updated server via Linux commands
* Installed and started Apache web server
* Served a webpage using EC2 Public IP

This is our **first DevOps-level practical** in server setup and web deployment.

