# 🔥 Advanced Linux Security: Exploitation & Hardening

Welcome to the **Ultimate Security Audit Framework** for Linux! 🛡️ This repository provides an automated solution for **monitoring, detecting, and mitigating security threats** on Linux systems.

---

## 🚀 Project Scope
This security automation tool helps you:
- 🔍 **Monitor user login attempts** (`last`, `auth.log`)
- ⚙️ **Detect and analyze running services** (`systemctl`)
- 📊 **Assess disk usage** (`df -h`)
- 🚨 **Trigger real-time alerts for unauthorized SSH access**

---

## 🛠️ Step 1: Deploy the Security Audit Script

### 📌 1️⃣ Create & Edit the Bash Script
Run this command to create the script file:

```bash
┌──(root㉿linux)-[~]
└─$ nano security_audit.sh
```

Copy and paste the following into the editor:

```bash
#!/bin/bash
# Linux Security Audit Script 🚨
LOG_FILE="/var/log/auth.log"  # On some systems, use /var/log/secure
ALERT_EMAIL="admin@example.com"

# 📝 Log user login history
echo "====== User Login History ======"
last -n 10
echo ""

# 🚔 Detect unauthorized SSH access
echo "====== Unauthorized SSH Access ======"
grep "Failed password" $LOG_FILE | tail -n 10
echo ""

# 🛠️ Inspect running services
echo "====== Active Services ======"
systemctl list-units --type=service --state=running | tail -n 20
echo ""

# 💾 Check disk space usage
echo "====== Disk Space Usage ======"
df -h | grep "^/dev"
echo ""

# 🚨 Alert on SSH brute-force attempts
if grep -q "Failed password" $LOG_FILE; then
    echo "[SECURITY ALERT] Unauthorized SSH login detected!" | mail -s "Critical Security Alert" $ALERT_EMAIL
fi
```

---

## 🛠️ Step 2: Execute the Security Audit Script

Make the script executable:
```bash
chmod +x security_audit.sh
```

Run the script:
```bash
./security_audit.sh
```

Example Output:
```
====== User Login History ======
root     pts/0    192.168.1.5    Mon Mar 18 10:45 - 10:50  (00:05)
...

====== Unauthorized SSH Access ======
Mar 18 11:10:21 linux sshd[1234]: Failed password for root from 192.168.1.10 port 5402 ssh2
...

====== Active Services ======
apache2.service      loaded active running The Apache HTTP Server
ssh.service          loaded active running OpenSSH server daemon
...

====== Disk Space Usage ======
/dev/sda1        50G  20G  30G  40% /
...
```

---

## 🔄 Step 3: Automate Security Checks with Cron

### 🕒 Schedule the script to run every 10 minutes:
```bash
crontab -e
```

Add the following line:
```bash
*/10 * * * * /path/to/security_audit.sh >> /var/log/security_audit.log 2>&1
```

Verify the scheduled task:
```bash
crontab -l
cat /var/log/security_audit.log
```

---

## 📌 Task Summary

| **Step**  | **Task** | **Command/Details** |
|-----------|---------|--------------------|
| **1️⃣ Create the Script** | Develop a security audit script | `nano security_audit.sh` |
| **2️⃣ Add Security Checks** | Track user logins, SSH attempts, active services, and disk usage | See script above |
| **3️⃣ Make Executable** | Allow script execution | `chmod +x security_audit.sh` |
| **4️⃣ Run Script** | Execute manually | `./security_audit.sh` |
| **5️⃣ Automate via Cron** | Run script every 10 mins | `crontab -e` + cron entry |
| **6️⃣ Verify Output** | Check logs | `cat /var/log/security_audit.log` |
| **7️⃣ Threat Analysis** | Review failed logins & anomalies | **Monitor script output** |
| **8️⃣ Apply Mitigation** | Strengthen SSH, disable unused accounts | `sudo passwd -l user` <br> `sudo nano /etc/ssh/sshd_config` |

---

### 🛡️ Final Thoughts
✅ **Deploy this script today and enhance your Linux security posture!**

🚀 Happy Hardening! 🔒
