# 🧠 Topic 4 – Bash Scripting (Introduction & Variables)

## 📘 Overview
Bash scripting is used to **automate tasks** in Linux systems.  
It helps DevOps engineers save time, reduce manual work, and run repetitive commands automatically.

---

## ⚙️ Create & Run a Script
1. Create a new file:
   ```bash
   nano myscript.sh
````

2. Add this line at the top:

   ```bash
   #!/bin/bash
   ```
3. Make it executable:

   ```bash
   chmod +x myscript.sh
   ```
4. Run it:

   ```bash
   ./myscript.sh
   ```

---

## 🧮 Variables

Store data inside scripts for reuse.

```bash
name="DevOps"
echo "Hello, $name!"
```

**Special variables:** `$0` = script name, `$1` = first argument, `$#` = total arguments.

---

## 💬 User Input & Arguments

```bash
echo "Enter your name:"; read name
echo "Welcome, $name!"
```

or

```bash
echo "First arg: $1"
./myscript.sh DevOps
```

---

## ⚖️ Conditions

```bash
if [ $num -gt 0 ]; then
  echo "Positive"
else
  echo "Negative or Zero"
fi
```

---

## 🔁 Loops

**For Loop**

```bash
for i in {1..5}; do echo $i; done
```

**While Loop**

```bash
i=1; while [ $i -le 5 ]; do echo $i; ((i++)); done
```

---

## 🧩 Common Commands

`echo` → Print text
`ls` → List files
`cd` → Change directory
`rm` → Delete file

---

## 💻 Practical Scripts

**Backup Script**

```bash
cp -r /source /backup/backup_$(date +%F_%T)
```

**Disk Usage Check**

```bash
usage=$(df / | awk 'NR==2 {print $5}' | sed 's/%//')
[ $usage -gt 80 ] && echo "High usage!" || echo "Normal"
```

---

## ✅ Summary

Bash scripting = foundation of automation in DevOps.
It makes server management, deployments, and monitoring **faster and smarter**
