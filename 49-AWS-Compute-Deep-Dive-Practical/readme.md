# AWS Compute Deep Dive – From Scratch

## 🚀 Real-Life Introduction

Tumhara ek idea hai — ek chhoti si "To-Do List" app banani hai.
AWS ke Compute Services help karte hain:

* Manual control chahiye? 👉 **EC2**
* Auto serverless chahiye? 👉 **Lambda**
* Simple app deploy karni hai fast? 👉 **Elastic Beanstalk**

---

## ☁️ AWS Compute Services Overview

* **EC2:** Virtual server, full control, SSH access
* **Lambda:** Serverless, pay-per-execution, trigger-based
* **Elastic Beanstalk:** PaaS, app upload, auto scaling, monitoring

---

## 🖥 EC2 Practical Steps (Ubuntu 20.04)

1. Launch EC2 (t2.micro, Security Group: SSH 22, HTTP 80)
2. SSH Access & Install Tools:

```bash
ssh -i key.pem ubuntu@<ec2-ip>
sudo apt update && sudo apt install -y git python3-pip awscli
```

3. AWS CLI Configure:

```bash
aws configure
# Enter Access Key, Secret Key, Region, Output format
```

---

## 🐍 Flask App Deployment via Elastic Beanstalk

1. Create Flask App:

```bash
mkdir flaskapp && cd flaskapp
nano application.py
# Paste Flask code
```

2. Create requirements & runtime files:

```bash
echo "flask" > requirements.txt
echo "gunicorn" >> requirements.txt
echo "python-3.9" > runtime.txt
```

3. Install EB CLI & Initialize:

```bash
pip3 install --upgrade --user awsebcli
eb init -p python-3.9 flaskapp --region us-east-1
eb create flask-env
eb open
```

4. Check Logs if RED:

```bash
eb logs
```

---

## 🛠 GitHub Integration

```bash
git init
git remote add origin https://github.com/shan376/flaskapp.git
git add .
git commit -m "Initial Flask app for Elastic Beanstalk deployment"
git branch -M main
git push -u origin main --force
```

Optional: Add `.gitignore` to exclude temp files

---

## ⚡ AWS Lambda Practical Assignment

1. Lambda Console → Create Function → Author from Scratch
2. Name: `HelloLambda`, Runtime: Python 3.9, Role: basic permissions
3. Paste Code:

```python
def lambda_handler(event, context):
    return {'statusCode': 200, 'body': 'Hello from AWS Lambda!'}
```

4. Deploy & Test → Output:

```json
{"statusCode":200,"body":"Hello from AWS Lambda!"}
```

Optional: Setup Function URL for public access

---

## 🧠 Summary (Ek Nazar Mein)

* **EC2:** Full control, manual server setup
* **Lambda:** Serverless, event-driven functions
* **Elastic Beanstalk:** Fully managed app deployment
* Har use-case ke liye alag service: Control = EC2, App deploy = Beanstalk, Small automated = Lambda

---

