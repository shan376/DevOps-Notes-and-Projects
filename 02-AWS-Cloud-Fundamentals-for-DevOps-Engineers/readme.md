## ☁️ Cloud Computing
Cloud computing means accessing data and applications **over the internet** instead of local machines.  
It provides on-demand services like **servers, storage, and databases** without managing hardware.  
**Examples:** Google Drive • AWS EC2 • Dropbox  

---

## 🔹 Cloud Models
**Public Cloud:** Shared services by providers (AWS, Azure, GCP).  
**Private Cloud:** Used by one organization (e.g., banking systems).  
**Hybrid Cloud:** Combines both for flexibility.  

---

## 🧱 Cloud Service Models
- **IaaS:** Virtualized hardware (AWS EC2, GCE) – user manages OS/apps.  
- **PaaS:** Platform tools for developers (Elastic Beanstalk, Heroku).  
- **SaaS:** Ready-to-use software (Gmail, Salesforce).  
> *DevOps mainly uses IaaS.*

---

## 🌍 AWS Global Infrastructure
AWS provides **high availability, low latency**, and **disaster recovery** via:  
- **Regions:** Global locations (e.g., `us-east-1`, `ap-south-1`)  
- **Availability Zones:** Isolated data centers within a region  
- **Edge Locations:** Content caching via CloudFront/CDN  

---

## 🚀 Common AWS Use Cases
Website Hosting • App Development • Analytics • Machine Learning • Backups • CI/CD Pipelines  

---

## 🔧 Core AWS Services for DevOps
**Compute:** EC2, Auto Scaling, ELB  
**Storage:** S3, EBS  
**Networking:** VPC, Subnets, Security Groups  
**Access:** IAM (Users, Roles, Policies)  

---

## 🧠 DevOps-Specific AWS Tools
| Service | Function |
|----------|-----------|
| CodePipeline | CI/CD Automation |
| CodeCommit | Source Control |
| CodeBuild | Build & Test |
| CodeDeploy | Deployment |
| CloudWatch | Monitoring |
| CloudFormation | Infrastructure as Code |

---

## 🧰 Tools Installed on EC2
Git • Jenkins • Docker • Ansible • Kubernetes (Minikube/K3s)

---

## 🪪 Create AWS Account
1. Go to [aws.amazon.com](https://aws.amazon.com/) → “Create an Account”  
2. Enter email, password, and personal info.  
3. Add payment (for verification – Free Tier valid 12 months).  
4. Verify OTP → Choose **Basic (Free)** plan → Sign in.  

---

## 💻 Launch Linux EC2 Instance
1. Open **EC2 Dashboard → Launch Instance**  
2. Select OS (Amazon Linux 2 / Ubuntu) & type `t2.micro`  
3. Create/download key pair → e.g., `mykey.pem`  
4. Allow **SSH (22)** from your IP → Launch  
5. Connect via SSH:

