# **Ansible in DevOps**

Ansible is an open-source automation tool used for **configuration management**, **application deployment**, and **task automation**.
It helps IT teams automate repetitive tasks like software installation, system updates, and cloud resource management.

Ansible is **agentless**, meaning no software is required on managed machines.
It uses **YAML-based Playbooks** to define automation tasks in a simple, human-readable format.

---

## **🔹 Key Uses in DevOps**

* Deploying applications
* Configuring servers
* Automating updates and patches
* Managing cloud environments

---

## **🧩 Ansible Basics (Part 1)**

* **Configuration Management:** Automates consistent configuration across servers.
* **Inventory File:** Lists managed nodes using INI or YAML format.
* **Playbooks:** YAML files defining tasks to be executed on target servers.

**Example Playbook:**

```yaml
---
- hosts: webservers
  become: yes
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present
```

---

## **⚙️ Ansible Advanced (Part 2)**

* **Roles:** Modular way to organize tasks, variables, and templates.
* **Variables:** Make playbooks reusable and dynamic.
* **Templates (Jinja2):** Generate configuration files dynamically using variables.

**Example Role Structure:**

```
my_role/
├── tasks/
├── vars/
├── templates/
├── defaults/
└── meta/
```

---

## **🚀 Summary**

* Ansible simplifies DevOps automation through agentless configuration.
* Basic tasks include creating inventory files and playbooks.
* Advanced features like **Roles**, **Variables**, and **Templates** make Ansible modular and reusable.

