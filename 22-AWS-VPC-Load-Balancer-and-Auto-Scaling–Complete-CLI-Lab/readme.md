# 🧩 AWS VPC, Load Balancer & Auto Scaling (CLI Lab)

### 🔹 VPC Basics

VPC (Virtual Private Cloud) AWS me ek private network hota hai jahan hum apni IP range, subnets, aur routing control karte hain.
Socho ye aapki apni chhoti city hai AWS cloud ke andar — ek public area (internet se connected) aur ek private area (secure & internal).

**Practical Summary:**

* Ek VPC create karte hain jisme public aur private subnets hoti hain.
* Public subnet me Internet Gateway (IGW) attach karte hain.
* Route tables set karte hain takay traffic sahi jagah jaye.
* EC2 instances launch karte hain dono subnets me — public server ping hota hai, private nahi.

---

### 🔹 Load Balancer & Auto Scaling

Load Balancer traffic ko evenly multiple servers par distribute karta hai.
Auto Scaling load ke mutabiq naye servers start ya band karta hai automatically.

**Real Example:**
Socho ek restaurant hai — Load Balancer waiter hai jo customers ko tables pe baantta hai, aur Auto Scaling staff hai jo zarurat ke mutabiq naye tables lagata ya hata deta hai.

**Practical Summary:**

1. Do EC2 instances launch karte hain jisme Apache install hota hai.
2. Target Group banate hain jahan servers register hote hain.
3. Application Load Balancer (ALB) create karte hain jo traffic ko distribute karta hai.
4. Launch Template aur Auto Scaling Group set karte hain taake load barhne par naye servers auto create ho jayein.
5. Stress tool se load simulate karke scaling test karte hain.

---

### 🔹 Replacement Check Commands

Agar VPC setup already ho, to ye commands se IDs check kar sakte ho 👇

* **AMI dekhne ke liye:**
  `aws ec2 describe-images --owners 099720109477 --region us-east-1`
* **Key Pair dekhne ke liye:**
  `aws ec2 describe-key-pairs --query "KeyPairs[*].KeyName" --output table`
* **Security Group:**
  `aws ec2 describe-security-groups --query "SecurityGroups[?IpPermissions[?ToPort==\`80`]].{ID:GroupId,Name:GroupName}" --output table`
* **Subnet ID:**
  `aws ec2 describe-subnets --query "Subnets[*].{Subnet_ID:SubnetId,AZ:AvailabilityZone}" --output table`
* **VPC ID:**
  `aws ec2 describe-vpcs --query "Vpcs[*].VpcId" --output table`
* **Instances:**
  `aws ec2 describe-instances --query "Reservations[*].Instances[*].InstanceId" --output text`
* **Target Group ARN:**
  `aws elbv2 describe-target-groups --query "TargetGroups[*].{Name:TargetGroupName,ARN:TargetGroupArn}" --output table`

---

**Summary:**

* **VPC** = aapki private city in AWS
* **Load Balancer** = traffic ka manager
* **Auto Scaling** = smart system jo load ke hisaab se servers manage karta hai
