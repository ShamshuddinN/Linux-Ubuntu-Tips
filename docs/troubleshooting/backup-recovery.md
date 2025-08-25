# 💾 Ubuntu Backup & Recovery Guide

> **Protect your data and system with comprehensive backup strategies and disaster recovery procedures**

## 📋 Table of Contents

- [🚀 Quick Backup Setup](#-quick-backup-setup)
- [📁 File & Data Backup](#-file--data-backup)
- [💿 System Image Backup](#-system-image-backup)
- [🔄 Automated Backup Solutions](#-automated-backup-solutions)
- [🛡️ System Recovery](#️-system-recovery)
- [☁️ Cloud Backup Integration](#️-cloud-backup-integration)
- [🗂️ Backup Best Practices](#️-backup-best-practices)
- [🚨 Emergency Recovery](#-emergency-recovery)

---

## 🚀 Quick Backup Setup

### Essential Backup in 10 Minutes

```bash path=null start=null
# Install backup tools
sudo apt update && sudo apt install rsync timeshift deja-dup

# Create system snapshot with Timeshift
sudo timeshift --create --comments "Initial system backup"

# Backup home directory
rsync -av --progress /home/$USER/ /media/backup/home-backup/

# Setup automated backups
sudo systemctl enable timeshift-scheduler
```

### Backup Checklist

- [ ] Important documents and files
- [ ] Configuration files (dotfiles)
- [ ] System settings and installed packages
- [ ] Application data and preferences
- [ ] Database backups (if applicable)
- [ ] SSH keys and certificates

---

## 📁 File & Data Backup

### Using rsync for File Backup

#### Basic File Sync

```bash path=null start=null
# Backup home directory to external drive
rsync -av --progress /home/$USER/ /media/backup/home/

# Backup with compression
rsync -avz --progress /home/$USER/ /media/backup/home/

# Exclude certain files/directories
rsync -av --progress --exclude='Downloads' --exclude='.cache' \
      /home/$USER/ /media/backup/home/

# Mirror backup (delete files that don't exist in source)
rsync -av --delete /home/$USER/ /media/backup/home/
```

#### Advanced rsync Options

```bash path=null start=null
# Backup with detailed logging
rsync -av --progress --log-file=/home/$USER/backup.log \
      /home/$USER/ /media/backup/home/

# Backup with bandwidth limit (KB/s)
rsync -av --progress --bwlimit=1000 \
      /home/$USER/ /media/backup/home/

# Backup over SSH to remote server
rsync -av --progress /home/$USER/ user@remotehost:/backup/home/

# Resume interrupted backup
rsync -av --progress --partial \
      /home/$USER/ /media/backup/home/
```

### Using tar for Archives

```bash path=null start=null
# Create compressed backup archive
tar -czf backup-$(date +%Y%m%d).tar.gz /home/$USER

# Create backup excluding certain directories
tar --exclude='/home/$USER/.cache' \
    --exclude='/home/$USER/Downloads' \
    -czf home-backup-$(date +%Y%m%d).tar.gz /home/$USER

# Extract backup archive
tar -xzf backup-20241225.tar.gz

# List contents of archive without extracting
tar -tzf backup-20241225.tar.gz | head -20
```

### Backup Script Creation

```bash path=null start=null
# Create automated backup script
cat > ~/backup-script.sh << 'EOF'
#!/bin/bash
# Personal backup script

# Configuration
SOURCE_DIR="/home/$USER"
BACKUP_DIR="/media/backup"
DATE=$(date +%Y%m%d_%H%M%S)
LOG_FILE="$HOME/backup-$DATE.log"

# Create backup directory if it doesn't exist
mkdir -p "$BACKUP_DIR"

# Start backup
echo "Starting backup at $(date)" | tee -a "$LOG_FILE"

# Rsync backup with exclusions
rsync -av --progress --delete \
      --exclude='.cache' \
      --exclude='Downloads' \
      --exclude='.local/share/Trash' \
      --exclude='.thumbnails' \
      "$SOURCE_DIR/" "$BACKUP_DIR/home/" 2>&1 | tee -a "$LOG_FILE"

# Backup package list
dpkg --get-selections > "$BACKUP_DIR/installed-packages.txt"

# Backup repository list
cp /etc/apt/sources.list "$BACKUP_DIR/"
cp -r /etc/apt/sources.list.d "$BACKUP_DIR/"

echo "Backup completed at $(date)" | tee -a "$LOG_FILE"
EOF

# Make executable
chmod +x ~/backup-script.sh

# Run backup script
~/backup-script.sh
```

---

## 💿 System Image Backup

### Using Timeshift (System Snapshots)

#### Installation and Setup

```bash path=null start=null
# Install Timeshift
sudo apt install timeshift

# Launch Timeshift GUI
sudo timeshift-gtk

# Or use command line
sudo timeshift --list
sudo timeshift --create --comments "Pre-update backup"
```

#### Timeshift Configuration

```bash path=null start=null
# Configure snapshot location
sudo timeshift --snapshot-device /dev/sdb1

# Set up automatic snapshots
sudo timeshift --create-filter
echo "/home" | sudo tee -a /etc/timeshift/filters.list

# Enable scheduled snapshots
cat > /etc/cron.daily/timeshift << 'EOF'
#!/bin/bash
/usr/bin/timeshift --create --scripted --comments "Daily automated backup"
EOF

chmod +x /etc/cron.daily/timeshift
```

### Using Clonezilla for Full Disk Imaging

#### Create Bootable Clonezilla

1. **Download Clonezilla ISO**: https://clonezilla.org/downloads.php
2. **Create bootable USB**:
   ```bash path=null start=null
   sudo dd if=clonezilla-live.iso of=/dev/sdX bs=4M status=progress
   ```

#### Disk Imaging Process

1. **Boot from Clonezilla USB**
2. **Choose "device-image" mode**
3. **Select source disk and destination**
4. **Choose compression level**
5. **Start imaging process**

### Using dd for Low-level Backup

> ⚠️ **Warning**: dd is powerful but dangerous. Double-check device names!

```bash path=null start=null
# Create full disk image
sudo dd if=/dev/sda of=/media/backup/disk-image.img bs=4M status=progress

# Create compressed disk image
sudo dd if=/dev/sda bs=4M status=progress | gzip > /media/backup/disk-image.img.gz

# Restore from disk image
sudo dd if=/media/backup/disk-image.img of=/dev/sda bs=4M status=progress

# Clone disk to another disk
sudo dd if=/dev/sda of=/dev/sdb bs=4M status=progress
```

---

## 🔄 Automated Backup Solutions

### Using Deja Dup (Duplicity Backend)

```bash path=null start=null
# Install Deja Dup
sudo apt install deja-dup

# Launch GUI
deja-dup

# Command line backup
duplicity /home/$USER file:///media/backup/duplicity

# Restore from backup
duplicity restore file:///media/backup/duplicity /home/$USER/restored
```

### Using BorgBackup

#### Installation and Setup

```bash path=null start=null
# Install BorgBackup
sudo apt install borgbackup

# Initialize repository
borg init --encryption=repokey /media/backup/borg-repo

# Create backup
borg create /media/backup/borg-repo::backup-{now:%Y-%m-%d-%H%M%S} \
     /home/$USER --exclude='*/Downloads' --exclude='*/.cache'

# List backups
borg list /media/backup/borg-repo

# Mount backup for browsing
mkdir /tmp/borg-mount
borg mount /media/backup/borg-repo::backup-2024-12-25-120000 /tmp/borg-mount
```

#### BorgBackup Script

```bash path=null start=null
# Create BorgBackup script
cat > ~/borg-backup.sh << 'EOF'
#!/bin/bash
# BorgBackup automation script

# Repository location
export BORG_REPO="/media/backup/borg-repo"

# Backup name with timestamp
BACKUP_NAME="backup-$(date +%Y-%m-%d-%H%M%S)"

# Create backup
borg create \
    --verbose \
    --filter AME \
    --list \
    --stats \
    --show-rc \
    --compression lz4 \
    --exclude-caches \
    --exclude '/home/*/.cache' \
    --exclude '/home/*/.thumbnails' \
    --exclude '/home/*/Downloads' \
    ::$BACKUP_NAME \
    /home/$USER

# Prune old backups
borg prune \
    --list \
    --prefix backup- \
    --show-rc \
    --keep-daily 7 \
    --keep-weekly 4 \
    --keep-monthly 6

echo "Backup completed: $BACKUP_NAME"
EOF

chmod +x ~/borg-backup.sh
```

### Scheduled Backups with Cron

```bash path=null start=null
# Edit user crontab
crontab -e

# Add backup schedules
# Daily backup at 2 AM
0 2 * * * /home/$USER/backup-script.sh

# Weekly system snapshot on Sunday at 3 AM
0 3 * * 0 /usr/bin/timeshift --create --scripted

# Monthly full system backup
0 4 1 * * /home/$USER/full-backup.sh

# View current cron jobs
crontab -l
```

---

## 🛡️ System Recovery

### Timeshift System Restore

#### From Running System

```bash path=null start=null
# List available snapshots
sudo timeshift --list

# Restore from specific snapshot
sudo timeshift --restore --snapshot '2024-12-25_10-00-01'

# Restore with GUI
sudo timeshift-gtk
```

#### From Live USB

1. **Boot from Ubuntu Live USB**
2. **Install Timeshift in live environment**:
   ```bash path=null start=null
   sudo apt update && sudo apt install timeshift
   ```
3. **Mount your system partition**
4. **Restore using Timeshift GUI**

### File Recovery

#### Recover Deleted Files

```bash path=null start=null
# Install file recovery tools
sudo apt install testdisk photorec extundelete

# Recover files with extundelete (ext3/ext4)
sudo extundelete /dev/sda1 --restore-file /path/to/deleted/file

# Recover all deleted files
sudo extundelete /dev/sda1 --restore-all

# Use PhotoRec for comprehensive recovery
photorec /dev/sda1
```

### GRUB Recovery

```bash path=null start=null
# Boot from Live USB and mount system
sudo mkdir /mnt/system
sudo mount /dev/sda1 /mnt/system
sudo mount /dev/sda2 /mnt/system/boot  # if separate boot partition

# Bind system directories
sudo mount --bind /dev /mnt/system/dev
sudo mount --bind /proc /mnt/system/proc
sudo mount --bind /sys /mnt/system/sys

# Chroot into system
sudo chroot /mnt/system

# Reinstall GRUB
grub-install /dev/sda
update-grub

# Exit chroot and reboot
exit
sudo reboot
```

---

## ☁️ Cloud Backup Integration

### Google Drive Integration

```bash path=null start=null
# Install rclone for cloud backups
sudo apt install rclone

# Configure Google Drive
rclone config

# Sync files to Google Drive
rclone sync /home/$USER/Documents gdrive:Documents

# Mount Google Drive as filesystem
mkdir ~/gdrive
rclone mount gdrive: ~/gdrive --daemon
```

### Dropbox Integration

```bash path=null start=null
# Install Dropbox
wget -O - "https://www.dropbox.com/download?plat=lnx.x86_64" | tar xzf -
~/.dropbox-dist/dropboxd

# Or use rclone for Dropbox
rclone config  # Configure Dropbox
rclone sync /home/$USER/Documents dropbox:Documents
```

### Amazon S3 Backup

```bash path=null start=null
# Install AWS CLI
sudo apt install awscli

# Configure AWS credentials
aws configure

# Sync to S3 bucket
aws s3 sync /home/$USER/Documents s3://your-bucket-name/Documents

# Using rclone for S3
rclone config  # Configure S3
rclone sync /home/$USER/Documents s3:your-bucket/Documents
```

---

## 🗂️ Backup Best Practices

### 3-2-1 Backup Strategy

**3** copies of important data
**2** different storage media types
**1** copy stored off-site

```bash path=null start=null
# Example implementation:
# Copy 1: Original data on SSD
# Copy 2: Local backup on external HDD
rsync -av /home/$USER/ /media/external-hdd/backup/

# Copy 3: Cloud backup (off-site)
rclone sync /home/$USER/ gdrive:backup/
```

### Backup Verification

```bash path=null start=null
# Verify backup integrity with checksums
find /home/$USER -type f -exec md5sum {} \; > /media/backup/checksums.md5

# Verify backup after restore
md5sum -c /media/backup/checksums.md5

# Compare directories
diff -r /home/$USER/ /media/backup/home/

# Use rsync for verification
rsync -av --dry-run --checksum /home/$USER/ /media/backup/home/
```

### Backup Testing

```bash path=null start=null
# Create test restore directory
mkdir /tmp/restore-test

# Test file restoration
rsync -av /media/backup/home/Documents/important.txt /tmp/restore-test/

# Test system snapshot restoration in VM
# Regular testing prevents backup surprises!
```

### Configuration Backup

```bash path=null start=null
# Backup system configuration
cat > ~/config-backup.sh << 'EOF'
#!/bin/bash
# System configuration backup

BACKUP_DIR="/media/backup/config"
mkdir -p "$BACKUP_DIR"

# Package lists
dpkg --get-selections > "$BACKUP_DIR/installed-packages.txt"
snap list > "$BACKUP_DIR/snap-packages.txt"

# Repository sources
cp /etc/apt/sources.list "$BACKUP_DIR/"
cp -r /etc/apt/sources.list.d "$BACKUP_DIR/"

# Network configuration
cp -r /etc/NetworkManager/system-connections "$BACKUP_DIR/"

# User configurations
cp ~/.bashrc "$BACKUP_DIR/"
cp ~/.profile "$BACKUP_DIR/"
cp -r ~/.ssh "$BACKUP_DIR/" 2>/dev/null || true
cp -r ~/.gnupg "$BACKUP_DIR/" 2>/dev/null || true

# Application configs
cp -r ~/.config "$BACKUP_DIR/" 2>/dev/null || true

echo "Configuration backup completed"
EOF

chmod +x ~/config-backup.sh
```

---

## 🚨 Emergency Recovery

### Boot Repair

```bash path=null start=null
# Install Boot Repair from Live USB
sudo add-apt-repository ppa:yannubuntu/boot-repair
sudo apt update && sudo apt install boot-repair

# Run Boot Repair
boot-repair

# Manual GRUB repair
sudo mount /dev/sda1 /mnt
sudo grub-install --root-directory=/mnt /dev/sda
```

### System Recovery from Backup

#### Complete System Restoration

```bash path=null start=null
# Boot from Live USB
# Format target drive (if needed)
sudo mkfs.ext4 /dev/sda1

# Mount target drive
sudo mkdir /mnt/restore
sudo mount /dev/sda1 /mnt/restore

# Restore from backup
sudo rsync -av /media/backup/system-image/ /mnt/restore/

# Install bootloader
sudo mount --bind /dev /mnt/restore/dev
sudo mount --bind /proc /mnt/restore/proc
sudo mount --bind /sys /mnt/restore/sys
sudo chroot /mnt/restore
grub-install /dev/sda
update-grub
exit

# Reboot to restored system
sudo reboot
```

### Data Recovery Services

When all else fails:
- **TestDisk/PhotoRec**: Free recovery tools
- **ddrescue**: For damaged drives
- **Professional services**: For critical data
- **Forensic tools**: For advanced recovery

---

## 📊 Backup Monitoring

### Backup Health Checking

```bash path=null start=null
# Create backup monitoring script
cat > ~/backup-monitor.sh << 'EOF'
#!/bin/bash
# Backup monitoring and alerting

BACKUP_DIR="/media/backup"
LOG_FILE="/var/log/backup-monitor.log"
EMAIL="your-email@example.com"

# Check if backup drive is mounted
if ! mountpoint -q "$BACKUP_DIR"; then
    echo "$(date): ERROR - Backup drive not mounted" | tee -a "$LOG_FILE"
    echo "Backup drive not accessible" | mail -s "Backup Alert" "$EMAIL"
    exit 1
fi

# Check backup age
LATEST_BACKUP=$(find "$BACKUP_DIR" -name "backup-*" -type d -printf "%T@ %p\n" | sort -nr | head -1)
BACKUP_AGE=$(echo "$LATEST_BACKUP" | cut -d' ' -f1)
CURRENT_TIME=$(date +%s)
AGE_DAYS=$(( (CURRENT_TIME - BACKUP_AGE) / 86400 ))

if [ $AGE_DAYS -gt 7 ]; then
    echo "$(date): WARNING - Latest backup is $AGE_DAYS days old" | tee -a "$LOG_FILE"
    echo "Backup is $AGE_DAYS days old" | mail -s "Backup Age Warning" "$EMAIL"
fi

# Check backup size
BACKUP_SIZE=$(du -sb "$BACKUP_DIR" | cut -f1)
if [ $BACKUP_SIZE -lt 1000000 ]; then  # Less than 1MB
    echo "$(date): ERROR - Backup size suspiciously small: $BACKUP_SIZE bytes" | tee -a "$LOG_FILE"
fi

echo "$(date): Backup monitor completed successfully" | tee -a "$LOG_FILE"
EOF

chmod +x ~/backup-monitor.sh

# Add to daily cron job
echo "0 6 * * * /home/$USER/backup-monitor.sh" | crontab -
```

---

## 🏆 Backup Strategy Templates

### Home User Strategy

```bash path=null start=null
# Documents and photos: Daily cloud sync
rclone sync ~/Documents gdrive:Documents
rclone sync ~/Pictures gdrive:Pictures

# System snapshot: Weekly
timeshift --create --comments "Weekly system backup"

# Full system backup: Monthly to external drive
rsync -av --delete / /media/external/system-backup/
```

### Developer Workstation

```bash path=null start=null
# Code repositories: Real-time sync to Git
git push origin main

# Development environment: Daily snapshot
timeshift --create --comments "Dev environment snapshot"

# Project backups: Automated with Borg
borg create ::projects-{now} ~/Projects ~/Development
```

### Server Environment

```bash path=null start=null
# Database backups: Daily
mysqldump --all-databases > /backup/mysql-$(date +%Y%m%d).sql

# Application data: Hourly incremental
rsync -av --link-dest=/backup/current /var/www/ /backup/$(date +%Y%m%d-%H%M%S)/

# System configuration: Daily
tar -czf /backup/config-$(date +%Y%m%d).tar.gz /etc
```

---

## 📚 Recovery Planning

### Create Recovery Documentation

```bash path=null start=null
# Create recovery instructions document
cat > ~/recovery-plan.md << 'EOF'
# System Recovery Plan

## Critical Information
- System: Ubuntu 22.04 LTS
- Backup Location: /media/backup
- Last Full Backup: [DATE]
- Emergency Contact: [EMAIL/PHONE]

## Recovery Steps
1. Boot from Ubuntu Live USB
2. Install Timeshift: sudo apt install timeshift
3. Mount backup drive: sudo mount /dev/sdb1 /media/backup
4. Restore system: sudo timeshift --restore
5. Verify system integrity
6. Update system: sudo apt update && sudo apt upgrade

## Important File Locations
- SSH Keys: ~/.ssh/
- GPG Keys: ~/.gnupg/
- Configurations: ~/.config/
- Documents: ~/Documents/

## Contact Information
- IT Support: [CONTACT]
- Data Recovery Service: [CONTACT]
EOF
```

---

> 💾 **Backup Philosophy**: The best backup is the one you actually use consistently. Start simple with basic file copying, then gradually implement more sophisticated strategies as your needs grow.

**Next Steps**: Now that your data is protected, explore our [OS Migration Guides](../getting-started/) for switching between systems, or check out [Essential Software](../customization/essential-software.md) recommendations.
