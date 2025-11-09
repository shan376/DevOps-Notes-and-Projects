# Terraform Basics with IAM User and EC2 Setup

## 📘 Concept

Terraform ek Infrastructure as Code (IaC) tool hai. Is se aap AWS ya dusre cloud resources ko code ke through automate kar sakte ho. Manual console se kaam karne ki bajaye, configuration file likh kar repeatable aur trackable infrastructure bana sakte ho.

---

## ⚙️ Practical Lab: Terraform + IAM + EC2

### 🟢 Step 1: Launch EC2 Instance

* Ubuntu 22.04 LTS
* Port 22 open karo (SSH)
* Instance type: t2.micro

```bash
# EC2 launch via AWS Console
```

---

### 🟢 Step 2: Install AWS CLI (agar apt install na ho)

```bash
sudo apt update
sudo apt install unzip -y
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version
```

---

### 🟢 Step 3: IAM User & Policy Setup

* AWS Console: IAM → Users → Add user
* Username: terraform-user
* Access type: Programmatic access
* Permissions: Attach AmazonEC2FullAccess
* Access Key aur Secret Key save kar lo

---

### 🟢 Step 4: Configure AWS CLI on EC2

```bash
aws configure
# Enter:
# AWS Access Key
# AWS Secret Key
# Default region (e.g., us-west-1)
# Output format (json)
```

---

### 🟢 Step 5: Install Terraform

```bash
sudo apt update
sudo apt install -y gnupg software-properties-common curl
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update
sudo apt install terraform -y
terraform -version
```

---

### 🟢 Step 6: Terraform Configuration File (main.tf)

```hcl
provider "aws" {
  region     = "us-west-1"
  access_key = "YOUR_ACCESS_KEY"
  secret_key = "YOUR_SECRET_KEY"
}

resource "aws_instance" "example" {
  ami           = "ami-0abcdef1234567890"
  instance_type = "t2.micro"
  tags = {
    Name = "TerraformInstance"
  }
}
```

> Note: Apne region ka sahi AMI ID AWS Console/CLI se nikal lo.

---

### 🟢 Step 7: Run Terraform Commands

```bash
terraform init   # initialize workspace
terraform plan   # changes check karo
terraform apply  # resource create karo, prompt pe yes type karo
terraform destroy # cleanup karna ho to
```

---

### 🧰 Common Errors & Solutions

| Error                        | Reason                           | Solution                        |
| ---------------------------- | -------------------------------- | ------------------------------- |
| InvalidAMIID.NotFound        | AMI ID galat ya region mismatch  | Correct AMI ID use karo         |
| UnauthorizedOperation        | IAM user ke paas permission nahi | AmazonEC2FullAccess attach karo |
| Package awscli not available | Ubuntu repo mein package nahi    | Manual install zip se karo      |
| list index out of range      | Region mismatch                  | Correct region specify karo     |

---

## 🧾 Assignment

1. Naya IAM user create karo jisme EC2 access permission ho
2. Terraform main.tf file mein IAM user keys use karo
3. Naya EC2 instance deploy karo apne desired region mein
4. AWS Console se verify karo ke instance bana hai ya nahi
5. Agar errors aaye, reason samjho aur note karo

---

### 📦 GitHub Push

```bash
git init
git add .
git commit -m "Terraform IAM + EC2 setup"
git branch -M main
git remote add origin https://github.com/<username>/terraform-iam-ec2.git
git push -u origin main
```

