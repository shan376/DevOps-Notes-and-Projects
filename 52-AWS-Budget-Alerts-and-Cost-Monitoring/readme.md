# 🚀 AWS IAM Advanced Concepts (Roles, Policies & Federation)

---

### 📘 Overview
AWS IAM (Identity and Access Management) provides **secure access control** through:
- **Roles** → Temporary credentials for AWS services/users  
- **Policies** → Permission rules written in JSON  
- **Federation** → External users (AD, Google, etc.) access AWS without IAM users  

---

### 🔑 Key Concepts

**1. IAM Roles**
- Temporary identity with limited permissions.  
- Used by EC2, Lambda, or cross-account access.  
- Example: EC2 reads from S3 without static keys.  
- 🧠 *Best Practice:* Always use least privilege.

**2. IAM Policies**
- JSON document defining “who can do what”.  
- Types:
  - **Managed:** AWS prebuilt  
  - **Inline:** Custom & specific  
- Example: Finance team = read-only, Dev = upload only.  

**3. Federation**
- Allows existing org users (Google AD, SSO) to access AWS.  
- Uses **SAML 2.0 / OIDC** standards.  
- Example: Employees login via corporate account — no IAM user needed.  

---

### 🧩 Practical Work — Cross Account S3 Access via Role

**Goal:**  
Allow **Account B** to securely read an **S3 bucket in Account A** using `sts:AssumeRole` (temporary credentials).

**Steps Summary:**
1. **Account A**
   - Create S3 bucket.  
   - Create IAM Role (trust = Account B).  
   - Attach `AmazonS3ReadOnlyAccess`.  
2. **Account B**
   - Create policy to `sts:AssumeRole`.  
   - Create IAM user with that policy.  
   - Configure AWS CLI.  
   - Run `aws sts assume-role` to get temporary creds.  
3. **Test**
   - Use assumed credentials to `aws s3 cp` file from Account A bucket.  

✅ Success = File copies via temporary access (no permanent keys).  

---

### 🧠 Important Commands
```bash
aws sts assume-role \
  --role-arn arn:aws:iam::<AccountA-ID>:role/CrossAccountS3ReadRole \
  --role-session-name CrossAcctS3Test
aws s3 cp s3://<Bucket-Name>/readme.txt ./ --profile cross-account
````

---

### 🧹 Clean Up

* Delete S3 bucket & role (Account A)
* Delete test user (Account B)
* Remove CLI profiles

---

### 🧠 Summary

| Concept            | Description                               |
| ------------------ | ----------------------------------------- |
| Roles              | Temporary identity for secure access      |
| Policies           | JSON-based permission control             |
| Federation         | External user access via SSO              |
| Cross Account Role | Enables secure S3 access between accounts |

---

**Assignment:**
Implement Cross-Account S3 Read Access using AssumeRole + temporary credentials for least-privilege security.

---

```
