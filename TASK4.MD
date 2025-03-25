# POC-TASKS
# 📌 Linux Security - Exploitation & Hardening (PoC)

This repository demonstrates **user and permission misconfigurations** in Linux, including **exploitation and mitigation**.

---

## 🔹 **Task 1: User & Permission Misconfigurations**

### ✅ **Setup: Creating Users & Misconfiguring Permissions**

```bash
# Security Audit Script (Kali Linux)
LOG_FILE="/var/log/auth.log"
OUTPUT_FILE="security_audit_report.txt"
EMAIL="admin@example.com"

# Kali Linux Prompt Style
PROMPT="┌──(kali㉿kali)-[~/Desktop]"

# Function to check user login attempts
echo "$PROMPT Checking User Login Attempts..." > $OUTPUT_FILE
last >> $OUTPUT_FILE
echo -e "\n$PROMPT Recent Authentication Logs:" >> $OUTPUT_FILE
grep "Failed\|Accepted" $LOG_FILE | tail -20 >> $OUTPUT_FILE

# Function to detect running services
echo -e "\n$PROMPT Detecting Running Services..." >> $OUTPUT_FILE
systemctl list-units --type=service --state=running >> $OUTPUT_FILE

# Function to monitor disk usage
echo -e "\n$PROMPT Monitoring Disk Usage..." >> $OUTPUT_FILE
df -h >> $OUTPUT_FILE

# Check for old user accounts (potential exploit)
echo -e "\n$PROMPT Checking for Old User Accounts..." >> $OUTPUT_FILE
awk -F: '$3 < 1000 {print $1}' /etc/passwd >> $OUTPUT_FILE

# Simulate Exploit - Check for users with empty passwords
echo -e "\n$PROMPT Users with Empty Passwords (Potential Exploit)..." >> $OUTPUT_FILE
awk -F: '($2=="!") || ($2=="*") {print $1}' /etc/shadow >> $OUTPUT_FILE

# Creating a test user 'CSK'
echo "$PROMPT Creating a test user 'CSK'..."
sudo useradd -m CSK && echo "CSK:password" | sudo chpasswd

# Mitigation: Setup a cron job for security monitoring (Kali Linux)
echo "$PROMPT Setting up security monitoring cron job..."
echo "* * * * * root /path/to/security_audit.sh | mail -s \"Security Alert\" $EMAIL" > /etc/cron.d/security_monitor

# Send alert if unauthorized SSH login attempts occur
echo "$PROMPT Checking for unauthorized SSH login attempts..."
grep "Failed password" $LOG_FILE | tail -5 | mail -s "Unauthorized SSH Attempt Alert" $EMAIL

echo "$PROMPT Security audit completed on Kali Linux. Report saved in $OUTPUT_FILE"
