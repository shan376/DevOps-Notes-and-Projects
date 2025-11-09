# Terraform + Ansible Infrastructure Automation

## Goal
Fully automate infrastructure provisioning and configuration from scratch using Terraform and Ansible.  
You will:
1. Launch EC2 using Terraform.
2. Automatically install Apache using Ansible.
3. Run everything with a single command: `terraform apply`.

## Concept
- **Terraform**: Provision EC2, VPC, Security Groups, Key Pairs.  
- **Ansible**: Configure servers, install Apache, deploy custom HTML.  

**Roman Urdu:**  
Terraform se EC2 aur resources create hotay hain, aur Ansible unko configure karta hai. Dono mil ke automation banatay hain.  

## Practical Steps

1. **Create EC2 (Ubuntu 22.04)**  
   - t2.micro, us-east-2, SSH + HTTP allowed  

2. **Install Required Tools**  
```bash
sudo apt update
sudo apt install -y unzip curl gnupg software-properties-common
````

3. **Install AWS CLI**

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws --version
```

4. **Install Terraform**

```bash
curl -fsSL https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update
sudo apt install terraform -y
terraform -version
```

5. **Install Ansible**

```bash
sudo apt update
sudo apt install -y ansible
ansible --version
```

6. **Configure AWS CLI**

```bash
aws configure
# Enter IAM user credentials + region (us-east-2) + output format json
```

7. **Setup SSH Key for Ansible**

```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa -N ""
cat ~/.ssh/id_rsa.pub
```

8. **Project Structure**

```
terraform-ansible/
├── main.tf
└── playbook.yml
```

* **main.tf** provisions EC2, Security Group, Key Pair, and runs Ansible playbook automatically.

9. **Ansible Playbook (playbook.yml)**

```yaml
- hosts: all
  become: yes
  tasks:
    - name: Install Apache
      apt:
        name: apache2
        state: present
        update_cache: yes

    - name: Upload custom index.html
      copy:
        content: "<h1>Hello from Terraform + Ansible</h1>"
        dest: /var/www/html/index.html
```

10. **Initialize and Apply**

```bash
terraform init
terraform plan
terraform apply
# Type yes to confirm
```

## Final Output

* EC2 instance created via Terraform.
* Apache installed automatically via Ansible.
* Custom HTML served at EC2 public IP.
* Test in browser: `http://<public-ip>` → "Hello from Terraform + Ansible"

## Assignment / Bonus

1. Create EC2 with Terraform.
2. Install Apache via Ansible.
3. Add your own custom webpage.
4. Test via EC2 public IP.
5. Push full project to GitHub for portfolio.

```
