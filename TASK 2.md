# 🌍 Linux Security: Remote Access & SSH Hardening

This repository covers **SSH misconfigurations**, their **exploitation**, and **best security practices** for hardening remote access in Linux. 🚀

---

## 🎯 Project Scope
This guide will help you:
- 🔍 **Understand SSH security risks**
- 🚨 **Exploit weak SSH configurations**
- 🛡 **Harden SSH against brute-force attacks**

---

## 🛠️ Step 1: Setting Up SSH with Weak Security

### 📌 **1️⃣ Install & Configure OpenSSH Server**
```bash
# Install OpenSSH Server
┌──(kali㉿kali)-[~]
└─$ sudo apt update && sudo apt install -y openssh-server

# Enable and start SSH
┌──(kali㉿kali)-[~]
└─$ sudo systemctl enable --now ssh

# Verify SSH is running
┌──(kali㉿kali)-[~]
└─$ sudo systemctl status ssh
```

### ⚠️ **2️⃣ Allow Root Login & Weak Authentication (Insecure)**
```bash
# Enable root login (HIGHLY INSECURE)
┌──(kali㉿kali)-[~]
└─$ sudo sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# Enable password authentication
┌──(kali㉿kali)-[~]
└─$ sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config

# Restart SSH service
┌──(kali㉿kali)-[~]
└─$ sudo systemctl restart ssh
```

---

## 🚀 Step 2: Exploiting Weak SSH Configurations

### 🔎 **3️⃣ Scan for Open SSH Ports**
```bash
# Check if SSH is listening on port 22
┌──(kali㉿kali)-[~]
└─$ ss -tlnp | grep ssh
```

### ⚔ **4️⃣ Brute-Force SSH Credentials using Hydra**
```bash
# Install Hydra (Brute-force tool)
┌──(kali㉿kali)-[~]
└─$ sudo apt install -y hydra

# Attempt SSH brute-force attack
┌──(kali㉿kali)-[~]
└─$ hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://127.0.0.1 -t 4
```

---

## 🔐 Step 3: Hardening SSH Security

### 🛡 **5️⃣ Disable Root Login & Password Authentication**
```bash
# Disable root login (SECURE)
┌──(kali㉿kali)-[~]
└─$ sudo sed -i 's/PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config

# Disable password authentication (Key-based login only)
┌──(kali㉿kali)-[~]
└─$ sudo sed -i 's/PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config

# Restart SSH
┌──(kali㉿kali)-[~]
└─$ sudo systemctl restart ssh
```

### 🔑 **6️⃣ Enable Key-Based Authentication**
```bash
# Generate SSH Key Pair
┌──(kali㉿kali)-[~]
└─$ ssh-keygen -t rsa -b 4096

# Copy Public Key to Remote System
┌──(kali㉿kali)-[~]
└─$ ssh-copy-id user@remote_host
```

---

### 🛡 **7️⃣ Prevent SSH Brute-Force Attacks using Fail2Ban**
```bash
# Install Fail2Ban
┌──(kali㉿kali)-[~]
└─$ sudo apt install -y fail2ban

# Enable and start Fail2Ban
┌──(kali㉿kali)-[~]
└─$ sudo systemctl enable fail2ban --now

# Configure Fail2Ban for SSH
┌──(kali㉿kali)-[~]
└─$ sudo nano /etc/fail2ban/jail.local
```
**Add the following configuration:**
```ini
[sshd]
enabled = true
port = 22
filter = sshd
logpath = /var/log/auth.log
maxretry = 3
bantime = 600
```
```bash
# Restart Fail2Ban service
┌──(kali㉿kali)-[~]
└─$ sudo systemctl restart fail2ban

# Check Fail2Ban status
┌──(kali㉿kali)-[~]
└─$ sudo fail2ban-client status sshd
```

---

## 📌 Task 2 - Summary Table

| **Step**  | **Action** | **Command/Details** |
|-----------|--------------------------------|-----------------------------|
| 🛠 **Setup** | Install & start SSH | `sudo apt install -y openssh-server` <br> `sudo systemctl enable --now ssh` |
| | Allow root login (INSECURE) | `sudo sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config` |
| | Enable password authentication (INSECURE) | `sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config` |
| 🚀 **Exploitation** | Check SSH status | `ss -tlnp | grep ssh` |
| | Brute-force SSH with Hydra | `hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://127.0.0.1 -t 4` |
| 🔐 **Mitigation** | Disable root login (SECURE) | `sudo sed -i 's/PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config` |
| | Enable key-based authentication | `ssh-keygen -t rsa -b 4096` <br> `ssh-copy-id user@remote_host` |
| | Disable password authentication | `sudo sed -i 's/PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config` |
| 🔥 **Prevent Brute-Force** | Install & configure Fail2Ban | `sudo apt install -y fail2ban` <br> `sudo nano /etc/fail2ban/jail.local` |

---

### 🛡 **Final Thoughts**
✅ **Implementing SSH hardening techniques significantly reduces the risk of unauthorized remote access. Always enforce strong authentication methods!**

🚀 Stay Secure! 🔒
