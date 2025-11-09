# Terraform Advanced – Reusable EC2 Module Lab

## ✅ Goal
Launch EC2 using a reusable Terraform module from a new Ubuntu EC2 instance, starting from IAM setup.

---

## 📘 Conceptual Overview

- **Modules**: Reusable infrastructure code, jaise programming mein functions. Ek dafa code likho aur baar-baar reuse karo.
- **State Management (.tfstate)**: Tracks existing resources aur unki state. Zaruri hai Terraform ko pata ho kya create/update/delete karna hai.
- **Remote State (S3)**: Team environments ke liye .tfstate ko S3 mein store karna secure, shared aur versioned rehta hai.

---

## 🔁 Real-Life Example
Agar 10 clients ke liye same EC2 setup chahiye:
```hcl
module "client1" {
  source = "./modules/ec2"
  ami    = "ami-xxxx"
}

module "client2" {
  source = "./modules/ec2"
  ami    = "ami-yyyy"
}
````

.tfstate file S3 mein store karne se backup aur team share safe rehta hai.

---

## 🗂️ Practical Lab Steps (Roman Urdu)

### Step 1: IAM User + Policy

* AWS Console → IAM → Create user `terraform-user`
* Programmatic access enable karo
* Policies attach: `AmazonEC2FullAccess`, optionally `AmazonS3FullAccess`
* Access Key ID & Secret Key securely save karo

### Step 2: Install AWS CLI on EC2

```bash
sudo apt update
sudo apt install unzip -y
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version
```

### Step 3: Configure AWS CLI

```bash
aws configure
# Enter IAM credentials
```

### Step 4: Install Terraform

```bash
sudo apt update
sudo apt install -y gnupg software-properties-common curl
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update
sudo apt install terraform -y
terraform -version
```

### Step 5: Create Terraform Module Project

```bash
mkdir terraform-project
cd terraform-project
mkdir -p modules/ec2-instance
touch main.tf modules/ec2-instance/ec2.tf modules/ec2-instance/variables.tf
```

**modules/ec2-instance/ec2.tf**

```hcl
resource "aws_instance" "myec2" {
  ami           = var.ami
  instance_type = var.instance_type
  tags = { Name = "ModularInstance" }
}
```

**modules/ec2-instance/variables.tf**

```hcl
variable "ami" {}
variable "instance_type" {}
```

**main.tf**

```hcl
provider "aws" {
  region = "us-east-2"
}

module "web" {
  source        = "./modules/ec2-instance"
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
}
```

### Step 6: Apply Terraform

```bash
terraform init
terraform plan
terraform apply
# Type yes when prompted
```

### Step 7: Folder Structure

```
terraform-project/
├── main.tf
└── modules/
    └── ec2-instance/
        ├── ec2.tf
        └── variables.tf
```

---

## ✅ Assignment

1. Launch EC2 (Ubuntu 22.04) in `us-east-2`, t2.micro, allow port 22
2. Install Terraform & AWS CLI on EC2
3. Create IAM user with EC2 & S3 access
4. Configure AWS CLI
5. Create S3 bucket for remote state with versioning
6. Setup Terraform modular project
7. Initialize, plan, apply Terraform
8. Verify EC2 instance creation and remote state storage

---

## ✅ Final Outcome

* EC2 instance created via module
* Remote state stored in S3
* IAM user access working
* No AMI or region mismatch errors

---

## 🧾 Checklist

| Task                              | Done? |
| --------------------------------- | ----- |
| IAM User Created + Configured     | ✅     |
| S3 Bucket Created with Versioning | ✅     |
| Folder Structure Modular          | ✅     |
| AMI & instance_type via module    | ✅     |
| Remote State Stored in S3         | ✅     |
| EC2 Instance Created with Tag     | ✅     |

```

