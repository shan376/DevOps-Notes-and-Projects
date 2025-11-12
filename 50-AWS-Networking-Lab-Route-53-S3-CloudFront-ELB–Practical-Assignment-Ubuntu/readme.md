# AWS Networking Lab – Route 53, S3, ELB, CloudFront

## Overview
Ye lab aapko sikhaega kaise AWS Networking services use karke website host karte hain, traffic manage karte hain aur global speed improve karte hain.  
Components:  
- **Route 53** – DNS management (Domain → IP)  
- **S3** – Static website hosting  
- **CloudFront** – CDN for global speed  
- **ELB** – Load balancer for traffic distribution  

---

## Lab 1: Static Website with S3 & Route 53

### Steps
1. **Domain ready karo** – GoDaddy ya kisi registrar se.  
2. **S3 Bucket create** – Bucket name = domain name, public access allow.  
3. **Upload website files** – index.html + assets, ensure public-read access.  
4. **Enable static website hosting** – Properties → Static website hosting → Index = index.html.  
5. **Bucket Policy set** – Public access JSON paste karo.  
6. **Route 53 Hosted Zone** – Domain add karo, type = Public hosted zone.  
7. **Create DNS record** – Type A, Alias = S3 endpoint.  
8. **Update domain registrar NS** – AWS nameservers GoDaddy me paste karo.  
9. **Test domain** – Browser me domain open karo, website load honi chahiye.  

✅ Result: S3 static website successfully domain ke saath linked hai, DNS verified.

---

## Lab 2: Load Balancer on EC2 (Ubuntu)

### Steps
1. **EC2 Instances** – 2 Ubuntu servers (Apache installed), HTTP 80 allowed.  
2. **Apache setup** – Start + enable, unique index.html content for each server.  
3. **Target Group create** – HTTP 80, dono EC2 register karo.  
4. **ALB create** – Internet-facing, HTTP listener, select AZs.  
5. **Attach Target Group** – ALB → Target Group attach karo.  
6. **Browser test** – EC2 individual test, phir ALB DNS open karo → traffic alternate servers pe.  

### Fix Healthy Status Issues
- Health Check Path = `/`, interval 30s, threshold 2  
- Security Groups – ALB 80 → 0.0.0.0/0, EC2 80 → ALB SG  
- Apache service check: `sudo systemctl status apache2`  
- index.html valid ho  
- Subnet/AZ same VPC me ho  

✅ Result: 2 Ubuntu EC2 instances running, ALB traffic distribute kar raha hai, High Availability achieved.

---

## Optional: CloudFront Setup
- CloudFront distribution create karo  
- Origin = S3 static website URL  
- Custom domain + SSL optional  

---

**Summary**
- Route 53 → Domain DNS  
- S3 → Static website hosting  
- ELB → Traffic distribution & High Availability  
- CloudFront → Global speed & caching  
