# IAM Management (Console + CLI)

### 🎯 Goal
Securely manage access to AWS resources using IAM via both Console and CLI.

---

## 🧠 What is IAM?
**IAM (Identity and Access Management)** lets you control who can access which AWS resources securely.

---

## ⚙️ IAM Components

| Component | Console Example | CLI Equivalent |
|------------|------------------|----------------|
| User | Individual login | `aws iam create-user` |
| Group | Collection of users | `aws iam create-group` |
| Policy | Rules defining permissions | `aws iam create-policy` |
| Role | Temporary access for services | `aws iam create-role` |

---

## 🧪 IAM Lab (CLI Based)

### Pre-requisite
AWS CLI must be configured (`aws configure`).

#### Step 1: Create IAM Group
```bash
aws iam create-group --group-name cli-group
````

#### Step 2: Attach Policy to Group

```bash
aws iam attach-group-policy \
--group-name cli-group \
--policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess
```

#### Step 3: Create IAM User

```bash
aws iam create-user --user-name cli-user
```

#### Step 4: Add User to Group

```bash
aws iam add-user-to-group \
--user-name cli-user \
--group-name cli-group
```

#### Step 5: Create Access Key

```bash
aws iam create-access-key --user-name cli-user
```

---

## 🔐 Least Privilege Lab (Custom Policy)

**Goal:** Allow only EC2 Start/Stop actions.

#### Step 1: Create User

```bash
aws iam create-user --user-name myiamuser
```

#### Step 2: Create Policy JSON

```bash
nano mypolicy.json
```

Paste:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["ec2:StartInstances", "ec2:StopInstances"],
      "Resource": "*"
    }
  ]
}
```

#### Step 3: Create Policy in AWS

```bash
aws iam create-policy --policy-name mypolicy --policy-document file://mypolicy.json
```

#### Step 4: Attach Policy to User

```bash
aws iam attach-user-policy --user-name myiamuser --policy-arn arn:aws:iam::<ACCOUNT_ID>:policy/mypolicy
```

#### Step 5: Generate Access Key

```bash
aws iam create-access-key --user-name myiamuser
```

#### Step 6: Configure AWS CLI Profile

```bash
aws configure --profile myiamuser
```

#### Step 7: Test Permissions

```bash
aws ec2 describe-instances --profile myiamuser   # ❌ Denied
aws ec2 start-instances --instance-ids <ID> --profile myiamuser   # ✅ Allowed
aws ec2 stop-instances --instance-ids <ID> --profile myiamuser    # ✅ Allowed
aws ec2 terminate-instances --instance-ids <ID> --profile myiamuser # ❌ Denied
```

---

## 🖥️ IAM via AWS Console

| Task                  | Console Steps                                   |
| --------------------- | ----------------------------------------------- |
| Create Group          | IAM → User groups → Create group                |
| Attach Policy         | Group → Permissions → Add policy                |
| Create User           | IAM → Users → Add user                          |
| Add User to Group     | User → Groups → Add to group                    |
| Create Access Key     | User → Security credentials → Create access key |
| Create Policy         | IAM → Policies → Create policy (JSON)           |
| Attach Policy to User | User → Permissions → Add policy                 |
| Configure CLI         | `aws configure --profile myiamuser`           
