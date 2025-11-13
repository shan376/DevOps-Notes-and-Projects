# 🚀 AWS Monitoring & Alarming  
**(Full Practical with IAM, KMS, EC2, CloudTrail, CloudWatch, SNS)**  

---

### 📘 Overview  
AWS Monitoring ka goal hai apne cloud resources ki **performance, health, aur security** ko continuously observe karna.  
Main services used:  
- **IAM** – User & permissions management  
- **KMS** – Data encryption key management  
- **EC2** – Application hosting (compute resource)  
- **CloudTrail** – Audit & API activity logging  
- **CloudWatch** – Metrics, dashboards & alarms  
- **SNS** – Notifications via email/SMS  

---

### ❓Why It’s Important  
- Security audit & compliance  
- Performance alerts & automation  
- Preventing failures before downtime  

---

### 🧩 Practical Lab Steps  

**1. IAM User (Admin Access)**  
Create IAM user → Console & programmatic access → Attach `AdministratorAccess` → Save credentials.  

**2. Create KMS Key**  
Go to KMS → Create symmetric key → Alias: `alias/cloudtrail-logs-key` → Add IAM user as admin →  
Use policy allowing CloudTrail to encrypt logs.  

**3. Launch EC2 Instance**  
EC2 → Launch Instance → Amazon Linux 2 → t2.micro → SSH (22) & HTTP (80) allowed → Launch & wait.  

**4. Enable CloudTrail (with KMS Encryption)**  
CloudTrail → Create Trail → New S3 bucket → Apply all regions →  
Select custom KMS key → Enable log validation → Create trail.  

**5. Create CloudWatch Dashboard**  
CloudWatch → Dashboards → Create → Add Line Graph → Metric: `EC2 → CPUUtilization` → Save dashboard.  

**6. Setup SNS Topic + Subscription**  
SNS → Create Topic (`CPU-Alerts`) → Subscription (Email) → Confirm from inbox.  

**7. Create CloudWatch Alarm (CPU > 70%)**  
CloudWatch → Alarms → Create alarm → Metric: `CPUUtilization` → Threshold >70% (5 min) →  
Action: Notify `CPU-Alerts` → Name: `High-CPU-Usage`.  

**8. Test Alarm using stress-ng**  
```bash
ssh -i key.pem ubuntu@<EC2-IP>
sudo apt update -y && sudo apt install stress-ng -y
stress-ng --cpu 4 --timeout 300s
stress-ng --cpu 2 --cpu-load 95 --timeout 600s
````

Watch alarm in CloudWatch → ALARM → OK → Check email for notifications.

**9. Verify CloudTrail Logs**
CloudTrail → Event History → Filter: `ec2.amazonaws.com` → View instance actions & user info.

---

### ✅ Summary

| Task        | Status | Notes                     |
| ----------- | ------ | ------------------------- |
| IAM User    | ✅      | Admin access created      |
| KMS Key     | ✅      | alias/cloudtrail-logs-key |
| EC2 Launch  | ✅      | t2.micro instance         |
| CloudTrail  | ✅      | Encrypted logs            |
| CloudWatch  | ✅      | CPUUtilization dashboard  |
| SNS + Alarm | ✅      | Email alerts working      |
| Stress Test | ✅      | Alarm tested successfully |

---

### 🧠 Assignment: CPU Spike Notification

Goal: Trigger **email alert** when EC2 CPU > 70%.
Follow Steps 1–4 (EC2 → SNS → CloudWatch → stress-ng).

---

💡 **End Result:**
You’ll have a complete AWS monitoring setup with **encrypted CloudTrail logs**, **CloudWatch alarms**, and **automated email alerts** via SNS.

---

