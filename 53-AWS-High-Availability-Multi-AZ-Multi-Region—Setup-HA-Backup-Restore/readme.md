# 🧭 AWS High Availability (HA) — Multi-AZ & Multi-Region Setup

### 📘 Overview

High Availability (HA) ensures your application remains available even during hardware or AZ failures.
AWS provides **Multi-AZ** and **Multi-Region** setups for maximum uptime, fault tolerance, and disaster recovery.

---

### ⚙️ Key Concepts

* **Multi-AZ** → Deploy app across multiple Availability Zones in the same region.
* **Multi-Region** → Replicate app across multiple AWS regions for global resilience.
* **Load Balancer (ALB/ELB)** → Distributes traffic evenly.
* **Auto Scaling** → Adjusts instance count based on load.
* **Bastion Host** → Secure SSH access to private instances.
* **NAT Gateway** → Enables internet access for private instances.

---

### 🧩 Architecture Summary

* **VPC** → Custom network (10.0.0.0/16)
* **Subnets** → 2 Public + 2 Private (Multi-AZ)
* **IGW & NAT** → Internet + Private access
* **EC2** → Bastion (Public) + Private EC2 (Web servers)
* **ALB** → Public traffic routed to private EC2s
* **Auto Scaling** → Ensures high availability

---

### 🔧 Practical Work

1. Create Custom VPC & Multi-AZ Subnets
2. Attach IGW and configure Route Tables
3. Setup **Bastion (Public)** + **Private EC2s (No Public IP)**
4. Add **NAT Gateway** for internet access
5. Install **Apache** on all instances
6. Configure **ALB** + **Target Group** for load balancing
7. Implement **Auto Scaling Group** for failover
8. Verify using ALB DNS (round-robin responses)

---

### 💾 Assignment — Backup & Restore Plan

**Objective:** Ensure continuous availability and disaster recovery.
**Steps:**

1. Create **AMI Backups** (instance-level).
2. Take **EBS Snapshots** (volume-level).
3. Backup app files to **S3** using IAM Role (no keys).
4. (Optional) Enable **Lifecycle Manager** for auto-snapshots.
5. Test restore from AMI, Snapshot, and S3.

---

### 🔑 Key Takeaways

* Multi-AZ → Local redundancy
* Multi-Region → Global redundancy
* ALB + ASG → Automated failover
* Bastion + NAT → Secure, production-grade access
* IAM Role → Secure S3 backup without keys
* Lifecycle Manager → Auto backups = zero data loss

---

**✅ Result:**
Fully functional, secure, and production-ready **Multi-AZ HA architecture** with Apache web servers, NAT, Bastion, Auto Scaling, and automated backup-restore capability.

---
