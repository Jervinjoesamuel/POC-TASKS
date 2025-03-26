# 🛡️ Linux Security - Advanced Log Analysis & Threat Detection

This repository explores **real-time log monitoring, anomaly detection, and intrusion prevention** in Linux. It focuses on **proactive defense mechanisms** and **log-based forensics** to identify and mitigate cyber threats. 🚀

---

## 🎯 Project Goals
This guide helps you:
- 📊 **Analyze system logs for anomalies**
- 🚨 **Detect unauthorized access attempts**
- 🛡 **Automate security monitoring with Fail2Ban & Logwatch**
- 🔍 **Investigate attacks using forensic techniques**

---

## 🛠️ Step 1: Activating & Verifying System Logging

### 📌 **1️⃣ Enable & Monitor System Logs**
```bash
# Enable system logging service
sudo systemctl enable systemd-journald
sudo systemctl restart systemd-journald

# Verify logging status
sudo journalctl --no-pager -n 20
```

### 🔎 **2️⃣ Locate Critical Security Logs**
```bash
# View authentication logs
sudo cat /var/log/auth.log | tail -20

# View system logs for suspicious activity
sudo journalctl -k -n 50
```

---

## 🚀 Step 2: Simulating & Detecting Unauthorized SSH Access

### ⚠️ **3️⃣ Trigger a Simulated Brute-Force Attack**
```bash
# Attempt multiple SSH logins with incorrect passwords
ssh root@localhost
# (Enter wrong passwords multiple times)
```

### 🛠 **4️⃣ Investigate Failed Login Attempts**
```bash
# Find repeated SSH failures
sudo grep "Failed password" /var/log/auth.log

# Identify suspicious login patterns
sudo journalctl -u sshd --no-pager | grep "Failed password"
```

### 📌 **5️⃣ Pinpoint Attackers by IP**
```bash
# Count failed logins per IP address
sudo grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr
```

---

## 🔐 Step 3: Strengthening Security with Automated Defenses

### 🛡 **6️⃣ Implement Fail2Ban to Block Intrusions**
```bash
# Install Fail2Ban
sudo apt install -y fail2ban

# Enable and start Fail2Ban service
sudo systemctl enable fail2ban --now
```

### 🔧 **7️⃣ Configure Fail2Ban for SSH Protection**
```bash
sudo nano /etc/fail2ban/jail.local
```
**Add the following configuration:**
```ini
[sshd]
enabled = true
bantime = 1200
maxretry = 3
logpath = /var/log/auth.log
```
```bash
# Restart Fail2Ban service
sudo systemctl restart fail2ban

# Check active Fail2Ban rules
sudo fail2ban-client status sshd
```

---

## 🔄 Step 4: Automating Security Log Analysis

### 📊 **8️⃣ Deploy Logwatch for Proactive Threat Reports**
```bash
# Install Logwatch
sudo apt install -y logwatch

# Generate a security report for SSH logs
sudo logwatch --detail high --logfile /var/log/auth.log --range today
```

### 📡 **9️⃣ Enable Real-Time Log Monitoring**
```bash
# Continuously watch logs for anomalies
sudo tail -f /var/log/auth.log
```

---

## 📌 Task Summary

| **Step**  | **Objective** | **Key Commands** |
|-----------|--------------------------------|-----------------------------|
| 🛠 **Enable Logging** | Activate system logging | `sudo systemctl enable systemd-journald` <br> `sudo systemctl restart systemd-journald` |
| 🔎 **Investigate Logs** | Detect unauthorized access | `grep "Failed password" /var/log/auth.log` <br> `journalctl -u sshd --no-pager | grep "Failed password"` |
| 🚨 **Identify Attackers** | Analyze brute-force attempts | `grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr` |
| 🛡 **Deploy Fail2Ban** | Block repeated failed logins | `sudo apt install fail2ban -y` <br> `sudo nano /etc/fail2ban/jail.local` |
| 🔄 **Automate Monitoring** | Generate security reports with Logwatch | `sudo apt install logwatch -y` <br> `sudo logwatch --detail high --logfile /var/log/auth.log --range today` |
| 📡 **Real-Time Defense** | Monitor logs live | `sudo tail -f /var/log/auth.log` |

---

### 🔥 **Final Thoughts**
✅ **Implementing log analysis, automated defenses, and real-time monitoring significantly enhances security visibility and threat mitigation.**

🚀 Stay Secure, Stay Vigilant! 🔒
