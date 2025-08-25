# 🛡️ Ubuntu Security & Privacy Guide

> **Protect your Ubuntu system with essential security practices and privacy enhancements**

## 📋 Table of Contents

- [🚀 Quick Security Setup](#-quick-security-setup)
- [🔥 Firewall Configuration](#-firewall-configuration)
- [🔐 User Account Security](#-user-account-security)
- [🔒 SSH Security](#-ssh-security)
- [📁 File Encryption](#-file-encryption)
- [🔍 System Monitoring](#-system-monitoring)
- [🌐 Network Security](#-network-security)
- [🛡️ Privacy Protection](#️-privacy-protection)
- [🔧 System Hardening](#-system-hardening)

---

## 🚀 Quick Security Setup

### Essential Security Commands (5 Minutes)

```bash path=null start=null
# Update system
sudo apt update && sudo apt upgrade -y

# Enable firewall
sudo ufw enable

# Install security tools
sudo apt install fail2ban ufw rkhunter clamav

# Set strong password policy
sudo apt install libpam-pwquality

# Enable automatic security updates
sudo apt install unattended-upgrades
sudo dpkg-reconfigure -plow unattended-upgrades
```

### Security Checklist

- [ ] System is up to date
- [ ] Firewall is enabled and configured
- [ ] Strong passwords are enforced
- [ ] SSH is properly secured
- [ ] Automatic security updates are enabled
- [ ] File permissions are properly set
- [ ] Unnecessary services are disabled

---

## 🔥 Firewall Configuration

### UFW (Uncomplicated Firewall)

#### Basic UFW Setup

```bash path=null start=null
# Check firewall status
sudo ufw status

# Enable firewall
sudo ufw enable

# Set default policies
sudo ufw default deny incoming
sudo ufw default allow outgoing

# Allow essential services
sudo ufw allow ssh              # SSH access
sudo ufw allow 80/tcp          # HTTP
sudo ufw allow 443/tcp         # HTTPS
sudo ufw allow 53              # DNS

# Check status with rules
sudo ufw status verbose
```

#### Advanced UFW Rules

```bash path=null start=null
# Allow specific applications
sudo ufw allow "Apache Full"
sudo ufw allow "Nginx Full"
sudo ufw allow "OpenSSH"

# Allow from specific IP addresses
sudo ufw allow from 192.168.1.100
sudo ufw allow from 192.168.1.0/24

# Allow specific ports
sudo ufw allow 8080/tcp
sudo ufw allow 1194/udp        # OpenVPN

# Deny specific connections
sudo ufw deny from 203.0.113.4
sudo ufw deny 21              # FTP

# Remove rules
sudo ufw delete allow 80/tcp
sudo ufw --numbered status     # Show numbered rules
sudo ufw delete 2              # Delete rule number 2
```

#### UFW Logging and Monitoring

```bash path=null start=null
# Enable logging
sudo ufw logging on

# Set logging level
sudo ufw logging medium        # off, low, medium, high, full

# View logs
sudo tail -f /var/log/ufw.log

# Reset firewall (remove all rules)
sudo ufw --force reset
```

### Advanced Firewall with iptables

```bash path=null start=null
# View current iptables rules
sudo iptables -L -n -v

# Save UFW rules to iptables format
sudo iptables-save > ~/firewall-backup.rules

# Block specific IP range
sudo iptables -I INPUT -s 192.168.1.0/24 -j DROP

# Limit SSH connections (prevent brute force)
sudo iptables -A INPUT -p tcp --dport 22 -m conntrack --ctstate NEW -m recent --set
sudo iptables -A INPUT -p tcp --dport 22 -m conntrack --ctstate NEW -m recent --update --seconds 60 --hitcount 4 -j DROP
```

---

## 🔐 User Account Security

### Password Policies

#### Install and Configure PAM

```bash path=null start=null
# Install password quality checking
sudo apt install libpam-pwquality

# Configure password policy
sudo nano /etc/security/pwquality.conf
```

Add these settings to `/etc/security/pwquality.conf`:

```ini path=/etc/security/pwquality.conf start=null
# Minimum password length
minlen = 12

# Require at least one digit
dcredit = -1

# Require at least one uppercase letter
ucredit = -1

# Require at least one lowercase letter
lcredit = -1

# Require at least one special character
ocredit = -1

# Maximum number of consecutive identical characters
maxrepeat = 3

# Reject passwords that contain the username
usercheck = 1
```

#### Password Aging

```bash path=null start=null
# Set password aging for specific user
sudo chage -M 90 username      # Password expires in 90 days
sudo chage -W 7 username       # Warning 7 days before expiration
sudo chage -I 30 username      # Account locked 30 days after expiration

# View password aging info
sudo chage -l username

# Force password change on next login
sudo chage -d 0 username
```

### Sudo Security

```bash path=null start=null
# Edit sudoers file safely
sudo visudo

# Add timeout for sudo (15 minutes default)
# Add this line to sudoers:
# Defaults timestamp_timeout=15

# Require password for sudo every time
# Defaults timestamp_timeout=0

# Log all sudo commands
# Add to sudoers:
# Defaults logfile="/var/log/sudo.log"
# Defaults log_input, log_output
```

### Account Lockout Policy

```bash path=null start=null
# Install fail2ban for intrusion prevention
sudo apt install fail2ban

# Configure fail2ban
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo nano /etc/fail2ban/jail.local

# Start and enable fail2ban
sudo systemctl enable fail2ban
sudo systemctl start fail2ban

# Check fail2ban status
sudo fail2ban-client status
sudo fail2ban-client status sshd
```

---

## 🔒 SSH Security

### SSH Server Hardening

```bash path=null start=null
# Backup SSH config
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup

# Edit SSH configuration
sudo nano /etc/ssh/sshd_config
```

#### Essential SSH Security Settings

```bash path=/etc/ssh/sshd_config start=null
# Change default port (security through obscurity)
Port 2222

# Disable root login
PermitRootLogin no

# Use SSH keys only
PasswordAuthentication no
PubkeyAuthentication yes

# Limit users who can SSH
AllowUsers yourusername

# Disable empty passwords
PermitEmptyPasswords no

# Connection settings
MaxAuthTries 3
LoginGraceTime 60
MaxStartups 3

# Disable X11 forwarding if not needed
X11Forwarding no

# Use strong ciphers
Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com
```

```bash path=null start=null
# Restart SSH service
sudo systemctl restart sshd

# Test SSH configuration
sudo sshd -t
```

### SSH Key Management

#### Generate SSH Keys

```bash path=null start=null
# Generate ED25519 key (recommended)
ssh-keygen -t ed25519 -C "your_email@example.com"

# Generate RSA key (if ED25519 not supported)
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

# Copy public key to server
ssh-copy-id -p 2222 user@server_ip

# Or manually copy public key
cat ~/.ssh/id_ed25519.pub | ssh user@server 'mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys'
```

#### SSH Key Security

```bash path=null start=null
# Set proper permissions
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
chmod 600 ~/.ssh/authorized_keys

# Use SSH agent for key management
eval $(ssh-agent -s)
ssh-add ~/.ssh/id_ed25519

# Add to shell profile for automatic loading
echo 'eval $(ssh-agent -s)' >> ~/.bashrc
echo 'ssh-add ~/.ssh/id_ed25519' >> ~/.bashrc
```

---

## 📁 File Encryption

### LUKS Full Disk Encryption

#### Check if LUKS is already enabled

```bash path=null start=null
# Check mounted encrypted partitions
lsblk -f

# View LUKS devices
sudo cryptsetup status /dev/mapper/*
```

#### Encrypt External Drives

```bash path=null start=null
# WARNING: This will destroy all data on the device
# Replace /dev/sdX with your actual device

# Create LUKS partition
sudo cryptsetup luksFormat /dev/sdX

# Open encrypted partition
sudo cryptsetup luksOpen /dev/sdX encrypted_drive

# Create filesystem
sudo mkfs.ext4 /dev/mapper/encrypted_drive

# Mount encrypted drive
sudo mkdir /mnt/encrypted
sudo mount /dev/mapper/encrypted_drive /mnt/encrypted

# Unmount and close
sudo umount /mnt/encrypted
sudo cryptsetup luksClose encrypted_drive
```

### File-level Encryption with GnuPG

```bash path=null start=null
# Install GnuPG (usually pre-installed)
sudo apt install gnupg

# Generate GPG key
gpg --full-generate-key

# List keys
gpg --list-keys

# Encrypt file
gpg --encrypt --recipient your_email@example.com sensitive_file.txt

# Decrypt file
gpg --decrypt sensitive_file.txt.gpg > sensitive_file.txt

# Encrypt with symmetric key (password)
gpg --symmetric --cipher-algo AES256 file.txt
```

### EncFS for Directory Encryption

```bash path=null start=null
# Install EncFS
sudo apt install encfs

# Create encrypted directory
mkdir ~/.encrypted
mkdir ~/.plaintext

# Initialize encrypted filesystem
encfs ~/.encrypted ~/.plaintext

# Mount encrypted directory
encfs ~/.encrypted ~/.plaintext

# Unmount
fusermount -u ~/.plaintext
```

---

## 🔍 System Monitoring

### System Integrity Monitoring

#### Install and Configure rkhunter

```bash path=null start=null
# Install rootkit hunter
sudo apt install rkhunter

# Initialize database
sudo rkhunter --propupd

# Run system check
sudo rkhunter --checkall --sk

# View logs
sudo cat /var/log/rkhunter.log

# Schedule regular checks
echo "0 3 * * * root /usr/bin/rkhunter --cronjob --report-warnings-only" | sudo tee -a /etc/crontab
```

#### Install ClamAV Antivirus

```bash path=null start=null
# Install ClamAV
sudo apt install clamav clamav-daemon

# Update virus definitions
sudo freshclam

# Scan home directory
clamscan -r --bell -i /home/

# Scan entire system (excluding mounted drives)
sudo clamscan -r --exclude-dir="^/sys" --exclude-dir="^/proc" --exclude-dir="^/dev" --bell -i /

# Schedule regular scans
echo "0 2 * * * root /usr/bin/clamscan -r --quiet --exclude-dir=\"^/sys\" --exclude-dir=\"^/proc\" --exclude-dir=\"^/dev\" --bell -i /" | sudo tee -a /etc/crontab
```

### Log Monitoring

```bash path=null start=null
# Monitor authentication logs
sudo tail -f /var/log/auth.log

# Monitor system logs
sudo tail -f /var/log/syslog

# Monitor kernel messages
sudo dmesg -T -f

# Search for failed login attempts
sudo grep "Failed password" /var/log/auth.log

# Monitor firewall logs
sudo tail -f /var/log/ufw.log

# Install logwatch for log analysis
sudo apt install logwatch
sudo logwatch --detail High --mailto your_email@example.com --service sshd
```

---

## 🌐 Network Security

### Network Scanning and Monitoring

```bash path=null start=null
# Install network tools
sudo apt install nmap netstat-nat ss

# Scan open ports on your system
sudo ss -tulpn
sudo netstat -tulpn

# Scan for open services
sudo nmap -sV localhost

# Monitor network connections
sudo ss -tuln

# Check listening services
sudo lsof -i -P -n | grep LISTEN
```

### Disable Unnecessary Services

```bash path=null start=null
# List all running services
systemctl list-units --type=service --state=running

# Disable unnecessary services (examples)
sudo systemctl disable bluetooth     # If not needed
sudo systemctl disable cups          # If not printing
sudo systemctl disable avahi-daemon  # If not needed

# Check service status
sudo systemctl status service_name

# Stop and disable service
sudo systemctl stop service_name
sudo systemctl disable service_name
```

### Network Hardening

```bash path=null start=null
# Disable IPv6 if not needed
echo 'net.ipv6.conf.all.disable_ipv6 = 1' | sudo tee -a /etc/sysctl.conf
echo 'net.ipv6.conf.default.disable_ipv6 = 1' | sudo tee -a /etc/sysctl.conf

# Enable TCP SYN flood protection
echo 'net.ipv4.tcp_syncookies = 1' | sudo tee -a /etc/sysctl.conf

# Disable source routing
echo 'net.ipv4.conf.all.accept_source_route = 0' | sudo tee -a /etc/sysctl.conf

# Disable ICMP redirect acceptance
echo 'net.ipv4.conf.all.accept_redirects = 0' | sudo tee -a /etc/sysctl.conf

# Apply changes
sudo sysctl -p
```

---

## 🛡️ Privacy Protection

### DNS Privacy

```bash path=null start=null
# Use secure DNS servers
sudo nano /etc/systemd/resolved.conf

# Add these lines:
# DNS=1.1.1.1 9.9.9.9
# FallbackDNS=8.8.8.8
# DNSOverTLS=yes

# Restart DNS resolver
sudo systemctl restart systemd-resolved

# Verify DNS over TLS
sudo systemd-resolve --status
```

### Browser Privacy

```bash path=null start=null
# Install privacy-focused browsers
sudo apt install firefox-esr       # Extended Support Release

# Install Tor Browser (manual installation)
wget https://www.torproject.org/dist/torbrowser/$(curl -s https://www.torproject.org/projects/torbrowser.html.en | grep -o 'torbrowser/[0-9.]*' | head -1 | cut -d'/' -f2)/tor-browser-linux64-$(curl -s https://www.torproject.org/projects/torbrowser.html.en | grep -o 'torbrowser/[0-9.]*' | head -1 | cut -d'/' -f2)_en-US.tar.xz

# Or install via snap
sudo snap install tor-browser-launcher
```

### Telemetry and Data Collection

```bash path=null start=null
# Disable Ubuntu telemetry (if present)
sudo apt remove ubuntu-report whoopsie apport

# Disable crash reporting
sudo systemctl disable apport.service

# Check for telemetry packages
dpkg -l | grep -E "(telemetry|report|crash|whoopsie)"

# Remove popularity contest
sudo apt remove popularity-contest
```

### Metadata Cleaning

```bash path=null start=null
# Install metadata removal tools
sudo apt install mat2 exiftool

# Remove metadata from files
mat2 --show file.jpg              # Show metadata
mat2 file.jpg                     # Remove metadata

# Using exiftool
exiftool -all= -overwrite_original file.jpg

# Bulk metadata removal
find ~/Pictures -name "*.jpg" -exec mat2 {} \;
```

---

## 🔧 System Hardening

### Kernel Security

```bash path=null start=null
# Install and configure AppArmor
sudo apt install apparmor apparmor-utils

# Check AppArmor status
sudo apparmor_status

# Enable AppArmor profiles
sudo aa-enforce /etc/apparmor.d/*

# Disable specific profile if needed
sudo aa-disable /etc/apparmor.d/usr.bin.firefox
```

### Secure Boot

```bash path=null start=null
# Check if Secure Boot is enabled
mokutil --sb-state

# If not enabled, enable in BIOS/UEFI settings
# Secure Boot helps prevent rootkit infections
```

### File System Security

```bash path=null start=null
# Set secure permissions on important files
sudo chmod 600 /etc/ssh/sshd_config
sudo chmod 600 /etc/shadow
sudo chmod 600 /etc/gshadow

# Remove world-readable permissions from home directory
chmod 750 $HOME

# Find world-writable files (potential security risk)
find / -type f -perm -002 2>/dev/null | grep -v /proc | grep -v /sys

# Find files with SUID bit set
find / -type f -perm -4000 2>/dev/null
```

### Automatic Security Updates

```bash path=null start=null
# Configure unattended upgrades
sudo apt install unattended-upgrades apt-listchanges

# Configure automatic updates
sudo dpkg-reconfigure -plow unattended-upgrades

# Edit configuration
sudo nano /etc/apt/apt.conf.d/50unattended-upgrades

# Enable automatic reboot if needed
# Uncomment and modify:
# Unattended-Upgrade::Automatic-Reboot "true";
# Unattended-Upgrade::Automatic-Reboot-Time "02:00";

# Test configuration
sudo unattended-upgrade --dry-run --debug
```

---

## 🚨 Security Incident Response

### Immediate Response Steps

```bash path=null start=null
# Disconnect from network (if compromised)
sudo ip link set down eth0    # Replace with your interface

# Check for suspicious processes
ps aux | grep -v "^\[" | sort -k3 -nr | head -20

# Check network connections
sudo ss -tuln
sudo lsof -i

# Check recent logins
last -n 20
lastlog

# Check system logs for anomalies
sudo grep -i "fail\|error\|warn" /var/log/syslog | tail -50
```

### System Recovery

```bash path=null start=null
# Boot from live USB and check system
# Mount infected system and scan
sudo mkdir /mnt/infected_system
sudo mount /dev/sda1 /mnt/infected_system

# Run antivirus scan from live environment
clamscan -r /mnt/infected_system

# Check for modified system files
sudo rkhunter --check --sk --propupd
```

---

## 📚 Security Tools Reference

### Essential Security Packages

```bash path=null start=null
# Install comprehensive security suite
sudo apt install \
  ufw fail2ban \
  rkhunter clamav \
  apparmor apparmor-utils \
  aide logwatch \
  chkrootkit lynis \
  nmap netstat-nat \
  gnupg2 encfs \
  mat2 exiftool
```

### Security Auditing

```bash path=null start=null
# Install Lynis for security auditing
sudo apt install lynis

# Run security audit
sudo lynis audit system

# View recommendations
sudo cat /var/log/lynis.log
```

---

## 🏆 Security Best Practices Summary

### Daily Security Habits

1. **Keep System Updated**
   ```bash path=null start=null
   sudo apt update && sudo apt upgrade
   ```

2. **Monitor Logs Regularly**
   ```bash path=null start=null
   sudo tail /var/log/auth.log
   ```

3. **Check Running Processes**
   ```bash path=null start=null
   ps aux | head -20
   ```

4. **Verify Network Connections**
   ```bash path=null start=null
   sudo ss -tuln
   ```

### Weekly Security Tasks

- [ ] Run full system antivirus scan
- [ ] Check for failed login attempts
- [ ] Review firewall logs
- [ ] Update virus definitions
- [ ] Backup important data

### Monthly Security Tasks

- [ ] Change critical passwords
- [ ] Review user accounts and permissions
- [ ] Update SSH keys if needed
- [ ] Run comprehensive security audit
- [ ] Test backup and recovery procedures

---

> 🔐 **Security Note**: Security is an ongoing process, not a one-time setup. Regular monitoring and maintenance are essential for keeping your system secure.

**Next Steps**: Implement these security measures gradually, starting with the Quick Security Setup. For ongoing monitoring, check out our [Performance Optimization](performance-optimization.md) guide to ensure security doesn't impact system performance.
