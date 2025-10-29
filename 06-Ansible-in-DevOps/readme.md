Perfect 👌
Here’s your **`README.md`** file for **Topic-07 — Ansible in DevOps**, written in a **short, professional, and recruiter-friendly GitHub format** — fully based on your provided data.

---

## 🧾 **Topic 07 — Ansible in DevOps**

### ⚙️ **Overview**

Ansible is an **open-source automation tool** used for configuration management, application deployment, and IT orchestration.
It helps DevOps engineers **automate repetitive tasks**, maintain **server consistency**, and **reduce manual errors** — all using simple **YAML-based Playbooks**.

---

### 🚀 **Key Features**

* Agentless (no installation needed on managed nodes)
* Uses **SSH** for communication
* **YAML Playbooks** define automation tasks
* Ideal for **server provisioning, deployment, and updates**
* Integrates smoothly with **CI/CD pipelines**

---

### 🧩 **Ansible Basics (Part 1)**

**Core Concepts:**

* **Configuration Management:** Maintain system consistency across environments
* **Inventory File:** Defines target servers (e.g., `inventory.ini`)
* **Playbooks:** YAML files that describe automation tasks

**Example:**

```yaml
# install_nginx.yml
---
- hosts: webservers
  become: yes
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present
```

**Run Command:**

```bash
ansible-playbook install_nginx.yml -i inventory.ini
```

---

### 🧠 **Ansible Advanced (Part 2)**

**1. Roles**

* Modular, reusable structure for organizing playbooks
* Typical directories: `tasks/`, `vars/`, `templates/`, `files/`, `meta/`

**2. Variables**

* Make playbooks **dynamic and reusable**
* Example:

  ```yaml
  vars:
    package_name: "nginx"
  ```

**3. Templates**

* Create **dynamic configuration files** using **Jinja2** syntax

  ```jinja2
  server {
    listen 80;
    server_name {{ server_name }};
    root {{ root_directory }};
  }
  ```

---

### 🧪 **Practical Work**

* Create a custom role using:

  ```bash
  ansible-galaxy init my_role
  ```
* Define tasks, variables, and templates
* Apply role in a playbook:

  ```yaml
  - hosts: webservers
    roles:
      - my_role
  ```
* Run:

  ```bash
  ansible-playbook -i inventory site.yml
  ```

---

### 🎯 **Assignments**

**1. Basic Playbook:** Automate Nginx installation on multiple servers
**2. Role-Based Project:** Deploy a simple web app using Nginx and HTML templates

---

### 🧾 **Summary**

| Concept                  | Description                          |
| ------------------------ | ------------------------------------ |
| Configuration Management | Automate system setup & maintenance  |
| Inventory File           | Defines managed servers              |
| Playbooks                | YAML-based automation scripts        |
| Roles                    | Modular organization for reusability |
| Templates                | Generate dynamic config files        |

📊 Diagram Reference

Diagrams are available in the diagram/ folder.
