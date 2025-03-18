# POC-TASKS
# 📌 Linux Security - Exploitation & Hardening (PoC)

This repository demonstrates **Remote Access (SSH) misconfigurations**, including **exploitation and mitigation**.

---

## 🔹 **Task 2: Remote Access & SSH Hardening**  

### ✅ **Setup: Enabling SSH & Allowing Root Login**  

```bash
# Update package list and install OpenSSH server
┌──(kali㉿kali)-[~]
└─$ sudo apt update && sudo apt install openssh-server -y

# Enable SSH to start on boot
┌──(kali㉿kali)-[~]
└─$ sudo systemctl enable --now ssh

# Verify SSH is running
┌──(kali㉿kali)-[~]
└─$ sudo systemctl status ssh

● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/usr/lib/systemd/system/ssh.service; enabled; preset: disa>
     Active: active (running) since Mon 2025-03-17 18:57:38 IST; 2h 1min ago
     -----

# Allow root login (INSECURE!)
┌──(kali㉿kali)-[~]
└─$ sudo sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# Enable password-based authentication (INSECURE!)
┌──(kali㉿kali)-[~]
└─$ sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config

# Restart SSH to apply changes
┌──(kali㉿kali)-[~]
└─$ sudo systemctl restart ssh

# Verify SSH is listening on port 22
┌──(kali㉿kali)-[~]
└─$ ss -tlnp | grep ssh
```

---

### ✅ **Exploitation: Brute-Force Attack on SSH**  

```bash
# Install Hydra (Brute-force tool)
┌──(kali㉿kali)-[~]
└─$ sudo apt install -y hydra

# Run Hydra brute-force attack on SSH
┌──(kali㉿kali)-[~]
└─$ hydra -l root -P passwords.txt ssh://178.0.0.9 -t 4
```

---

### ✅ **Mitigation: Hardening SSH Security**  

```bash
# Disable root login for SSH (SECURE)
┌──(kali㉿kali)-[~]
└─$ sudo sed -i 's/PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config

# Enable key-based authentication
┌──(kali㉿kali)-[~]
└─$ mkdir -p ~/.ssh && chmod 700 ~/.ssh

# Generate SSH key pair
┌──(kali㉿kali)-[~]
└─$ ssh-keygen -t rsa -b 4096

# Copy public key to authorized keys
┌──(kali㉿kali)-[~]
└─$ ssh-copy-id root@127.0.0.1

# Disable password authentication (SECURE)
┌──(kali㉿kali)-[~]
└─$ sudo sed -i 's/PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config

# Restart SSH to apply changes
┌──(kali㉿kali)-[~]
└─$ sudo systemctl restart ssh
```

---

### ✅ **Mitigation: Prevent Brute-Force Attacks with Fail2Ban**  

```bash
# Install Fail2Ban
┌──(kali㉿kali)-[~]
└─$ sudo apt install -y fail2ban

# Enable and start Fail2Ban
┌──(kali㉿kali)-[~]
└─$ sudo systemctl enable fail2ban
sudo systemctl start fail2ban

# Configure Fail2Ban for SSH protection
┌──(kali㉿kali)-[~]
└─$ sudo nano /etc/fail2ban/jail.local
```

Add the following content:

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
# Restart Fail2Ban to apply settings
┌──(kali㉿kali)-[~]
└─$ sudo systemctl restart fail2ban

# Check Fail2Ban status for SSH
┌──(kali㉿kali)-[~]
└─$ sudo fail2ban-client status sshd
```

---

## 📌 Task 2 - Summary  

| **Step**              | **Action**                                         | **Command**                                  |
|----------------------|-------------------------------------------------|---------------------------------------------|
| 🔹 **Setup**          | Install & start SSH                              | `sudo apt update && sudo apt install -y openssh-server`<br>`sudo systemctl enable ssh`<br>`sudo systemctl start ssh` |
|                      | Allow root login (INSECURE)                      | `sudo sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config` |
|                      | Enable password authentication (INSECURE)        | `sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config` |
|                      | Restart SSH to apply changes                     | `sudo systemctl restart ssh` |
| 🔹 **Exploitation**   | Install Hydra                                   | `sudo apt install -y hydra` |
|                      | Perform brute-force attack using Hydra          | `hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://<target-ip> -t 4` |
| 🔹 **Mitigation**     | Disable root login (SECURE)                     | `sudo sed -i 's/PermitRootLogin yes/PermitRootLogin no/' /etc/ssh/sshd_config` |
|                      | Enable key-based authentication                 | `mkdir -p ~/.ssh && chmod 700 ~/.ssh`<br>`ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa -N ""`<br>`cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys`<br>`chmod 600 ~/.ssh/authorized_keys` |
|                      | Disable password authentication (SECURE)        | `sudo sed -i 's/PasswordAuthentication yes/PasswordAuthentication no/' /etc/ssh/sshd_config` |
|                      | Restart SSH to apply changes                     | `sudo systemctl restart ssh` |
| 🔹 **Prevent Brute-Force** | Install Fail2Ban                        | `sudo apt install -y fail2ban` |
---

### ------------------------------------------THE END OF TASK 2---------------------------------------
