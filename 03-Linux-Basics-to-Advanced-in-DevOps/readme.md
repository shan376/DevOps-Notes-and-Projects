# 🐧 Topic 3 – Linux in DevOps (Basics to Advanced)

## 📘 Overview
Linux is the **core of DevOps environments** — powering servers, containers, and automation tools.  
It’s **open-source, secure, stable, and scriptable**, making it ideal for CI/CD pipelines and cloud systems.

---

## 🏁 1. History of Unix & Linux
- **Unix (1970s, AT&T Bell Labs):** Multi-user, multitasking OS written in C (portable).  
- **Linux (1991, Linus Torvalds):** Free, open-source Unix-like kernel.  
- Combined with GNU tools → **GNU/Linux**.  
- Today, Linux runs most **servers, cloud systems, and DevOps infrastructure**.

---

## ⚙️ 2. Why Linux is Crucial in DevOps
- Most tools (Docker, Jenkins, Kubernetes, Ansible, Git) run natively on Linux.  
- Offers **scripting & automation** (Bash, Shell).  
- Enables **infrastructure management**, deployments, and troubleshooting.

---

## 💡 3. Key Features of Linux
- 🧩 **Open Source:** Fully customizable, community-driven.  
- 👥 **Multi-user System:** Many users can work simultaneously.  
- ⚡ **Multitasking:** Run multiple jobs efficiently.  
- 🛡️ **Security & Stability:** Strong permissions and uptime.  
- 🌐 **Networking & Tool Support:** Built-in support for major DevOps tools.  
- 📦 **Package Management:** Install/manage software via `apt`, `yum`, or `dnf`.

---

## 🪟 4. Linux vs Windows (Quick Comparison)

| Feature | Linux | Windows |
|----------|--------|----------|
| Source Code | Open Source | Closed Source |
| Kernel Type | Monolithic | Hybrid |
| Interface | CLI + GUI | GUI Focused |
| Security | High | Moderate |
| Performance | Lightweight | Heavy |
| Cost | Free | Paid License |
| Ideal For | DevOps, Servers | Desktop Use |

---

## ✍️ 5. Creating & Managing Files

### 📄 File Creation
| Command | Purpose |
|----------|----------|
| `cat > file.txt` | Create/write file |
| `touch file.txt` | Create empty file |
| `vi file.txt` | Edit using vi editor |
| `nano file.txt` | Edit using nano editor |

### 🔍 File Operations
| Task | Command |
|------|----------|
| View content | `cat`, `less`, `head`, `tail` |
| Copy or Rename | `cp`, `mv` |
| Delete file | `rm file.txt` |
| Check details | `ls -l`, `stat file.txt` |

---

## 📁 6. Directory Operations

| Task | Command | Description |
|------|----------|-------------|
| Create directory | `mkdir devops` | Creates a directory |
| Change directory | `cd dirname` | Move into folder |
| List files | `ls -a` | Show all (hidden too) |
| Go back | `cd ..` | Move up one level |
| Show current path | `pwd` | Display working directory |
| Delete directory | `rm -r dirname` | Remove recursively |

---

## 💻 7. Essential Linux Commands (DevOps)

| Command | Description |
|----------|--------------|
| `hostname` | Show system name |
| `ip a` | Display network info |
| `cat /etc/os-release` | Show OS version |
| `yum install <pkg>` | Install a package |
| `service httpd start` | Start Apache |
| `chkconfig httpd on` | Enable service at boot |
| `whoami` | Show logged-in user |
| `grep "error" file.log` | Search specific text |
| `history` | List previously used commands |

---

## 🔐 8. File Permissions

### Permission Types
- **r (read):** View content  
- **w (write):** Modify content  
- **x (execute):** Run or enter directory  

### User Categories
- **Owner** – File creator  
- **Group** – Users in same group  
- **Others** – Everyone else  

### Numeric Values
| Number | Permission | Meaning |
|---------|-------------|----------|
| 7 | rwx | Full access |
| 6 | rw- | Read + Write |
| 5 | r-x | Read + Execute |
| 4 | r-- | Read only |
| 0 | --- | No access |

### Common Commands
```bash
chmod 754 file.txt      # rwx r-x r--
chmod a+rwx script.sh   # Give all full access
chown user:group file   # Change owner and group
ls -l                   # View permissions
````

---

## 🧠 9. User & Group Management

| Task              | Command                       | Purpose            |
| ----------------- | ----------------------------- | ------------------ |
| Add new user      | `useradd devuser`             | Create new user    |
| Create group      | `groupadd devgroup`           | Add a new group    |
| Add user to group | `gpasswd -a devuser devgroup` | Manage permissions |

---

## 🧰 10. Archiving & Download Utilities

| Command                    | Purpose              | Example           |
| -------------------------- | -------------------- | ----------------- |
| `tar -cvf backup.tar dir/` | Archive files        | Backup directory  |
| `gzip backup.tar`          | Compress file        | Reduce size       |
| `wget https://...`         | Download from web    | Get files via URL |
| `ln -s /var/log logs`      | Create symbolic link | Shortcut to path  |

---

## 🪪 11. Superuser Access (Root)

* Switch to root user:

  ```bash
  sudo su
  ```
* Prompt changes to `[root@ip]#`
* Root can modify all files and system configurations (⚠️ Use with care).

---

## ✅ Final Tips for DevOps Engineers

* Practice commands daily on **AWS EC2 (Ubuntu)**.
* Use `ls -l`, `chmod`, `chown` frequently.
* Automate repetitive tasks using **bash scripts**.
* Work as **normal user** unless `sudo` is required.
* Linux mastery = **DevOps foundation** 
