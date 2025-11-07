# 🧩 **AWS VPC Basics using CLI (DevOps Practical)**

### 🔹 **VPC Kya Hai?**

VPC (Virtual Private Cloud) ek AWS ke andar **private network** hota hai jahan hum apne servers aur services ko **securely run** karte hain.
Jaise apna ek **data center** hota hai, bas ye cloud ke andar hota hai jisme IPs, subnets, routing aur firewall sab apne control mein hote hain.

---

### 🎯 **Objective:**

CLI se ek **Custom VPC** banana jisme:

* 1 Public Subnet
* 1 Private Subnet
* Internet Gateway
* Public Route Table
* Aur ek EC2 Instance (optional)

---

### ⚙️ **Prerequisites:**

* Ubuntu EC2 instance running
* AWS CLI installed & configured
* IAM user/role with EC2 full access
* SSH access via PuTTY working

---

## 🚀 **VPC CLI Practical Steps (Short Summary)**

#### 🧱 Step 1: Create VPC

`aws ec2 create-vpc --cidr-block 10.0.0.0/16`
➡️ Ye command ek naya **VPC (network building)** banati hai.

#### 🌐 Step 2: Create Public Subnet

`aws ec2 create-subnet --vpc-id vpc-xxxx --cidr-block 10.0.1.0/24`
➡️ Ye **public subnet** banata hai jahan web server jaise resources aayenge.

#### 🔒 Step 3: Create Private Subnet

`aws ec2 create-subnet --vpc-id vpc-xxxx --cidr-block 10.0.2.0/24`
➡️ Ye **private subnet** hai — internal servers (e.g., database) ke liye.

#### 🌍 Step 4: Create Internet Gateway (IGW)

`aws ec2 create-internet-gateway`
➡️ Ye **internet access point** hai jo VPC ko internet se connect karega.

#### 🔗 Step 5: Attach IGW to VPC

`aws ec2 attach-internet-gateway --internet-gateway-id igw-xxxx --vpc-id vpc-xxxx`
➡️ Ab VPC ko internet se connect kar diya gaya.

#### 🗺️ Step 6: Create Route Table

`aws ec2 create-route-table --vpc-id vpc-xxxx`
➡️ Ye VPC ke andar traffic ka **map** banata hai.

#### 🚦 Step 7: Create Route to IGW

`aws ec2 create-route --route-table-id rtb-xxxx --destination-cidr-block 0.0.0.0/0 --gateway-id igw-xxxx`
➡️ Ye rule batata hai ke internet traffic **IGW ke through** jaaye.

#### 🔁 Step 8: Associate Route Table with Public Subnet

`aws ec2 associate-route-table --subnet-id subnet-xxxx --route-table-id rtb-xxxx`
➡️ Public subnet ko route table se link kar diya.

#### 🌐 Step 9: Enable Auto-Assign Public IP

`aws ec2 modify-subnet-attribute --subnet-id subnet-xxxx --map-public-ip-on-launch`
➡️ Ab har naya EC2 jo public subnet mein launch hoga, usko **public IP** mil jaayegi.

#### 💻 Step 10: Launch EC2 Instance

`aws ec2 run-instances --image-id ami-xxxx --count 1 --instance-type t2.micro --subnet-id subnet-xxxx --associate-public-ip-address`
➡️ Ye command ek **web server** launch karti hai jo internet se reachable hoga.

---

## 🧠 **VPC Summary (Roman Urdu Style)**

1. Nayi VPC banayi → ek secure network setup.
2. Do subnets banaye → public aur private.
3. IGW banaya aur VPC se attach kiya → internet access ke liye.
4. Route Table aur route create kiya → traffic direction ke liye.
5. Public subnet ko link kiya → taake wo internet se connect ho.
6. Public IP enable kiya → EC2 ko direct IP mil sake.
7. EC2 launch kiya → web server ready ho gaya.

---

### 🏗️ **Real-World Example:**

Jaise tum ek nayi **building** bana rahe ho (VPC),
do **kamre** (subnets) banaye — ek public, ek private,
ek **wifi router** lagaya (IGW),
map banaya ke kahan se network jaaye (route table),
aur ek **employee** rakha (EC2 instance) jo kaam kare.

