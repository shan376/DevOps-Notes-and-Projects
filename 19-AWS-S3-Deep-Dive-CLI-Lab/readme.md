# S3 Deep Dive (Full Final Notes with Labs)

## 📘 1. What is S3?

Amazon S3 (Simple Storage Service) is an object-based storage used to store files, backups, images, and logs in the cloud.
It stores data as objects inside buckets.

**Roman Urdu:**
S3 ek cloud-based drive hai jismein data “bucket” ke andar store hota hai. Jaise Google Drive mein folders hote hain, waise S3 mein buckets hote hain.

---

## ⚙️ 2. CLI Installation & Configuration

**Install AWS CLI (Ubuntu):**

```bash
sudo apt update
sudo apt install awscli -y
```

**Configure AWS CLI:**

```bash
aws configure
```

Enter:

* Access Key
* Secret Key
* Region (e.g. ap-south-1)
* Output format (json)

**Roman Urdu:**
`aws configure` se hum AWS CLI ko apne account ke sath connect karte hain.

---

## 🪣 3. Practical Work – Bucket Creation & Public Access

### Step 1: Create S3 Bucket

```bash
aws s3 mb s3://my-cli-bucket-devops
```

### Step 2: Disable Public Access Block

```bash
aws s3api put-public-access-block \
--bucket my-cli-bucket-devops \
--public-access-block-configuration BlockPublicAcls=false,IgnorePublicAcls=false,BlockPublicPolicy=false,RestrictPublicBuckets=false
```

### Step 3: Add Bucket Policy (Public Read)

```bash
aws s3api put-bucket-policy \
--bucket my-cli-bucket-devops \
--policy '{
"Version":"2012-10-17",
"Statement":[{
"Sid":"PublicReadGetObject",
"Effect":"Allow",
"Principal": "*",
"Action":["s3:GetObject"],
"Resource":["arn:aws:s3:::my-cli-bucket-devops/*"]
}]
}'
```

**Roman Urdu:**
Pehle public block hataate hain, phir policy lagate hain jisse har koi S3 object ko read kar sakta hai.

---

## 📤 4. Upload File to S3

```bash
echo "My DevOps File" > file.txt
aws s3 cp file.txt s3://my-cli-bucket-devops/
```

**Check Public URL:**

```
https://my-cli-bucket-devops.s3.amazonaws.com/file.txt
```

---

## 🧑‍💻 5. Assignment – Upload & Manage via CLI

```bash
aws s3 cp test.txt s3://my-cli-bucket-devops/
aws s3 ls s3://my-cli-bucket-devops/
```

---

## 🧪 6. Lab Task – Full Flow

```bash
echo "File data for testing" > file.txt
aws s3 cp file.txt s3://my-cli-bucket-devops/
aws s3 ls s3://my-cli-bucket-devops/
aws s3 cp s3://my-cli-bucket-devops/file.txt .
cat file.txt
```

**Roman Urdu:**
File ko download kar ke verify karte hain ke upload sahi hua hai ya nahi.

---

## ✅ Checklist

| Task                         | Status |
| ---------------------------- | ------ |
| CLI installed & configured   | ✅      |
| Bucket created via CLI       | ✅      |
| Public access set via policy | ✅      |
| File uploaded                | ✅      |
| Object URL verified          | ✅      |
| File downloaded & checked    | ✅      |

---

## 🪣 Conclusion

You learned to manage **S3 via CLI**:

* ✅ CLI Installation
* ✅ Bucket Creation
* ✅ Public Access Policy
* ✅ Upload & Download Verification

**Roman Urdu:**
Ye DevOps ke real-world use case ka part hai — Console ki jagah CLI use karna professional practice hoti hai.

