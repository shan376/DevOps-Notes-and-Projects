# 🧰 AWS CLI – Install, Configure & Lab Practice

### 🎯 **Goal**

Use AWS services (EC2, S3) from terminal using CLI with IAM authentication.

---

### ⚙️ **1. Install AWS CLI (Ubuntu)**

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip -y
unzip awscliv2.zip
sudo ./aws/install
aws --version
```

---

### 👤 **2. IAM Setup (User + Policy)**

**Create IAM User (Programmatic Access):**

1. IAM → Users → Add user → Name: `cli-user`
2. Select ✅ Programmatic access

**Create Custom Policy:**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:StartInstances",
        "ec2:StopInstances",
        "s3:*"
      ],
      "Resource": "*"
    }
  ]
}
```

**Policy Name:** `DevOpsCLIPolicy`
Attach this policy to `cli-user`.

---

### 🔑 **3. Configure AWS CLI**

```bash
aws configure
```

Enter:

* AWS Access Key ID
* AWS Secret Access Key
* Region (e.g., ap-south-1)
* Output format (json)

Config files:

```
~/.aws/credentials
~/.aws/config
```

---

### ☁️ **4. S3 Concept**

S3 (Simple Storage Service) is AWS’s object storage to store files, backups, and logs securely.

---

### 💻 **5. CLI Lab Practice**

**A. List EC2 Instances**

```bash
aws ec2 describe-instances
```

**B. List All S3 Buckets**

```bash
aws s3 ls
```

**C. Create New Bucket**

```bash
aws s3 mb s3://my-devops-bucket-172
```

**D. Create & Upload File**

```bash
cat > myfile.txt
aws s3 cp myfile.txt s3://my-devops-bucket-172/
```

**E. Verify Upload**

```bash
aws s3 ls s3://my-devops-bucket-172/
```

---

### 🧩 **6. Uninstall / Reinstall (Optional)**

```bash
sudo rm -rf /usr/local/aws-cli
sudo rm /usr/local/bin/aws
which aws
aws --version
sudo ./aws/install
```

---

### ✅ **Final Checklist**

* ✔ AWS CLI installed
* ✔ IAM user & custom policy created
* ✔ CLI configured with keys & region
* ✔ EC2 instances listed
* ✔ S3 bucket created & file uploaded
