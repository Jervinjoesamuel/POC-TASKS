# 🌐 Linux Security: Firewall & Network Hardening

This repository explores **firewall misconfigurations** and **network security risks** in Linux, focusing on **exploitation techniques** and **preventative security measures**. 🔥

---

## 🎯 Project Scope
This guide will help you:
- 🔍 **Set up and secure a web server**
- 🚨 **Identify and exploit open ports**
- 🛡 **Harden the system using UFW and iptables**

---

## 🛠️ Step 1: Setting Up a Web Server

### 📌 **1️⃣ Install and Configure Apache**
```bash
# Update system and install Apache web server
┌──(kali㉿kali)-[~]
└─$ sudo apt install -y apache2

# Enable Apache to start on boot
┌──(kali㉿kali)-[~]
└─$ sudo systemctl enable apache2

# Start Apache service
┌──(kali㉿kali)-[~]
└─$ sudo systemctl start apache2

# Verify Apache is running
┌──(kali㉿kali)-[~]
└─$ sudo systemctl status apache2
```

### ⚠️ **2️⃣ Disable UFW (Insecure Setup for Testing)**
```bash
┌──(kali㉿kali)-[~]
└─$ sudo ufw disable  
Firewall stopped and disabled on system startup
```

---

## 🚀 Step 2: Exploiting Firewall & Open Ports

### 🔎 **3️⃣ Scan for Open Ports**
```bash
# List active network connections
┌──(kali㉿kali)-[~]
└─$ sudo netstat -tulnp

# Scan for open ports using Nmap
┌──(kali㉿kali)-[~]
└─$ sudo nmap -sS -A localhost   
```
**Example Output:**
```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.9p1 Debian
80/tcp open  http    Apache httpd 2.4.63
```

### ⚔ **4️⃣ Exploit Misconfigured Open Ports**
```bash
# Check if HTTP (80) is accessible
┌──(kali㉿kali)-[~]
└─$ nc -zv localhost 80
localhost [178.0.0.9] 80 (http) open
```

---

## 🔐 Step 3: Hardening Firewall & Network Security

### 🛡 **5️⃣ Secure the System with UFW Firewall**
```bash
# Enable UFW
┌──(kali㉿kali)-[~]
└─$ sudo ufw enable

# Allow only SSH (22) and HTTP (80)
┌──(kali㉿kali)-[~]
└─$ sudo ufw allow 22/tcp
┌──(kali㉿kali)-[~]
└─$ sudo ufw allow 80/tcp

# Set default policies to block all incoming connections except allowed ones
┌──(kali㉿kali)-[~]
└─$ sudo ufw default deny incoming
┌──(kali㉿kali)-[~]
└─$ sudo ufw default allow outgoing

# Verify UFW rules
┌──(kali㉿kali)-[~]
└─$ sudo ufw status verbose  
```

### 🔥 **6️⃣ Strengthening Security with iptables**
```bash
# Allow only SSH & HTTP traffic
┌──(kali㉿kali)-[~]
└─$ sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
┌──(kali㉿kali)-[~]
└─$ sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT

# Save iptables rules
┌──(kali㉿kali)-[~]
└─$ sudo netfilter-persistent save
```

---

## 📌 Task 3 - Summary Table

| **Step**  | **Action** | **Command/Details** |
|-----------|--------------------------------|-----------------------------|
| 🛠 **Setup** | Install & start Apache | `sudo apt install -y apache2` <br> `sudo systemctl enable apache2` <br> `sudo systemctl start apache2` |
| | Verify Apache | `sudo systemctl status apache2` |
| | Disable UFW (for testing) | `sudo ufw disable` |
| 🚀 **Exploitation** | Check open ports | `sudo netstat -tulnp` |
| | Scan ports with Nmap | `sudo nmap -sS -A localhost` |
| | Verify accessible ports | `nc -zv localhost 80` |
| 🔐 **Mitigation (UFW)** | Enable firewall | `sudo ufw enable` |
| | Allow only SSH & HTTP | `sudo ufw allow 22/tcp` <br> `sudo ufw allow 80/tcp` |
| | Set strict default rules | `sudo ufw default deny incoming` <br> `sudo ufw default allow outgoing` |
| 🔥 **Mitigation (iptables)** | Restrict network traffic | `sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT` <br> `sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT` |
| | Save firewall rules | `sudo netfilter-persistent save` |

---

### 🛡 **Final Thoughts**
✅ **Securing network services and implementing firewall rules is essential for preventing unauthorized access. Always monitor and restrict network traffic to reduce attack surfaces.**

🚀 Stay Secure! 🔒
