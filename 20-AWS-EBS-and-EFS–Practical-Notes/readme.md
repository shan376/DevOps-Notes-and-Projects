# EBS & EFS

## ⚙️ EBS (Elastic Block Store)

EBS ek virtual hard drive hai jo sirf **ek EC2 instance** ke sath attach hoti hai.
Agar EC2 stop ya restart bhi ho jaye to data safe rehta hai.
Socho jaise laptop ki apni hard drive hoti hai — waise hi EBS EC2 ki personal drive hai.

---

## 🌐 EFS (Elastic File System)

EFS ek **shared storage** hai jise **multiple EC2 instances** ek saath access kar sakte hain.
Jaise office ka shared folder hota hai jahan sab log files read/write karte hain, waise hi EFS kaam karta hai.

---

## 💡 Real-Life Example

* **EBS = Personal Hard Drive (sirf ek machine ke liye)**
* **EFS = Shared Folder (sab machines ke liye)**

---

## 🧪 Practical Work (Short)

1. `aws ec2 create-volume` → naya 5GB EBS volume banao
2. `curl http://169.254.169.254/latest/meta-data/instance-id` → EC2 ID lo
3. `aws ec2 attach-volume` → volume ko EC2 se attach karo
4. `lsblk` → check karo volume laga hai ya nahi
5. `sudo mkfs -t ext4 /dev/xvdf` → format karo
6. `sudo mkdir /data && sudo mount /dev/xvdf /data` → mount karo
7. `aws ec2 create-snapshot` → backup snapshot lo

---

## ✅ Final Note

EBS ek single EC2 ke liye hoti hai (personal drive),
jabke EFS multiple EC2s ke liye (shared folder).
Dono ko milake production apps mein use kiya jata hai —
EBS system data ke liye, aur EFS shared files ke liye.

