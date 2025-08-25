# 🔄 Switching from Windows to Ubuntu

> **Complete guide to migrating from Windows to Ubuntu Linux with step-by-step instructions and tips for a smooth transition**

## 📋 Table of Contents

- [🚀 Pre-Migration Planning](#-pre-migration-planning)
- [💿 Creating Installation Media](#-creating-installation-media)
- [🔒 Preparing Your System](#-preparing-your-system)
- [💻 Ubuntu Installation Process](#-ubuntu-installation-process)
- [🔧 Post-Installation Setup](#-post-installation-setup)
- [📁 Data Migration](#-data-migration)
- [🎯 Windows Software Alternatives](#-windows-software-alternatives)
- [🛠️ Troubleshooting](#️-troubleshooting)

---

## 🚀 Pre-Migration Planning

### System Requirements Check

#### Minimum Requirements
- **Processor**: 2 GHz dual-core processor
- **Memory**: 4 GB RAM (8 GB recommended)
- **Storage**: 25 GB free hard drive space
- **Graphics**: VGA capable of 1024x768 resolution
- **Internet**: Required for updates and additional software

#### Hardware Compatibility Check

```bash path=null start=null
# After booting Ubuntu Live USB, check hardware compatibility:
# WiFi adapter
lspci | grep -i network

# Graphics card
lspci | grep -i vga

# Audio devices
lspci | grep -i audio

# USB devices
lsusb
```

### Data Backup Strategy

#### Essential Windows Data to Backup

```bash path=null start=null
# Create backup checklist:
# Documents: C:\Users\[Username]\Documents
# Desktop files: C:\Users\[Username]\Desktop
# Pictures: C:\Users\[Username]\Pictures
# Music: C:\Users\[Username]\Music
# Videos: C:\Users\[Username]\Videos
# Downloads: C:\Users\[Username]\Downloads
# Browser bookmarks and passwords
# Email data (Outlook PST files, Thunderbird profile)
# Application settings and license keys
```

#### Backup Methods

1. **External Drive Backup**
   - Copy important folders to external USB drive
   - Use Windows built-in backup tools

2. **Cloud Backup**
   - OneDrive, Google Drive, Dropbox
   - Ensures data accessibility from Ubuntu

3. **Network Backup**
   - Backup to network-attached storage (NAS)
   - Another computer on the network

---

## 💿 Creating Installation Media

### Download Ubuntu ISO

```bash path=null start=null
# Official Ubuntu download page: https://ubuntu.com/download/desktop
# Recommended: Ubuntu 22.04 LTS (Long Term Support)
# File size: ~4-5 GB
```

### Creating Bootable USB Drive

#### Using Rufus (Recommended for Windows)

1. **Download Rufus**: https://rufus.ie/en/
2. **Insert USB drive** (8GB or larger)
3. **Run Rufus as Administrator**
4. **Select Ubuntu ISO file**
5. **Choose USB device**
6. **Click START**

#### Alternative: Balena Etcher

1. **Download Balena Etcher**: https://www.balena.io/etcher/
2. **Flash Ubuntu ISO to USB**
3. **Verify the flash process completed**

### Testing Ubuntu Before Installation

```bash path=null start=null
# Boot from USB and select "Try Ubuntu"
# Test hardware functionality:
# - WiFi connectivity
# - Audio output
# - Graphics performance
# - External devices (printer, camera, etc.)
# - Bluetooth devices
```

---

## 🔒 Preparing Your System

### Windows Preparation Steps

#### 1. Disable BitLocker Encryption

**In Windows PowerShell (Admin mode):**

```powershell path=null start=null
# Check BitLocker status
manage-bde -status

# Disable BitLocker (replace C: with your drive)
Disable-BitLocker -MountPoint "C:"

# Wait for decryption to complete (can take hours)
manage-bde -status C:
```

#### 2. Disable Fast Startup

```bash path=null start=null
# In Windows:
# 1. Open Control Panel > Power Options
# 2. Click "Choose what the power buttons do"
# 3. Click "Change settings that are currently unavailable"
# 4. Uncheck "Turn on fast startup"
```

#### 3. Disable Secure Boot (if needed)

```bash path=null start=null
# In BIOS/UEFI settings:
# 1. Restart computer and press F2/F12/DEL (varies by manufacturer)
# 2. Navigate to Security settings
# 3. Disable Secure Boot
# 4. Enable Legacy Boot (if dual-booting)
# 5. Save and exit
```

#### 4. Create System Recovery Media

```bash path=null start=null
# In Windows:
# 1. Search "Create a recovery drive"
# 2. Follow the wizard to create recovery USB
# 3. Keep this safe for emergency Windows recovery
```

### Disk Space Planning

#### Single Boot (Ubuntu Only)

```bash path=null start=null
# Recommended partition scheme:
# / (root): 50-100 GB minimum
# /home: Remaining space for user data
# swap: 2x RAM size (if RAM < 8GB) or equal to RAM size
```

#### Dual Boot Setup

```bash path=null start=null
# Windows partition: Keep existing Windows partition
# Ubuntu partitions:
# / (root): 50 GB minimum
# /home: 100+ GB for user data
# swap: 4-8 GB
# Total Ubuntu space needed: 200+ GB recommended
```

---

## 💻 Ubuntu Installation Process

### Boot Configuration

#### BIOS Boot Order

```bash path=null start=null
# 1. Insert Ubuntu USB drive
# 2. Restart computer
# 3. Press F2/F12/ESC during startup (varies by manufacturer)
# 4. Set USB as first boot device
# 5. Save and restart
```

#### UEFI Boot Menu

```bash path=null start=null
# Alternative: One-time boot menu
# 1. Restart computer
# 2. Press F12 during startup (common key)
# 3. Select USB drive from boot menu
```

### Installation Steps

#### 1. Language and Keyboard

- Select your preferred language
- Choose appropriate keyboard layout
- Test keyboard layout with provided text box

#### 2. Updates and Other Software

```bash path=null start=null
# Recommended selections:
☑ Download updates while installing Ubuntu
☑ Install third-party software for graphics and Wi-Fi hardware
```

#### 3. Installation Type

**For Complete Ubuntu Installation:**
```bash path=null start=null
# Select: "Erase disk and install Ubuntu"
# This will remove Windows completely
```

**For Dual Boot:**
```bash path=null start=null
# Select: "Install Ubuntu alongside Windows"
# Or: "Something else" for manual partitioning
```

#### 4. Manual Partitioning (Advanced)

```bash path=null start=null
# Create partitions:
# 1. EFI System Partition (if UEFI): 512 MB
# 2. Root partition (/): ext4, 50-100 GB
# 3. Home partition (/home): ext4, remaining space
# 4. Swap partition: swap, 4-8 GB
```

#### 5. User Account Creation

```bash path=null start=null
# Create user account:
# - Your name
# - Computer name (hostname)
# - Username (lowercase, no spaces)
# - Password (strong password recommended)
# - Choose login options:
#   ☑ Require password to log in (recommended)
#   ☐ Log in automatically
```

#### 6. Installation Progress

```bash path=null start=null
# Installation time: 20-45 minutes depending on:
# - Hard drive speed (SSD faster than HDD)
# - Internet connection (for updates)
# - Computer specifications
```

---

## 🔧 Post-Installation Setup

### Initial System Update

```bash path=null start=null
# Update package lists and system
sudo apt update && sudo apt upgrade -y

# Install restricted extras for multimedia support
sudo apt install ubuntu-restricted-extras

# Reboot system
sudo reboot
```

### Essential Software Installation

#### Development Tools

```bash path=null start=null
# Install essential development packages
sudo apt install curl wget git vim build-essential

# Install snap package support (usually pre-installed)
sudo apt install snapd
```

#### Media Codecs

```bash path=null start=null
# Install multimedia codecs
sudo apt install ubuntu-restricted-extras

# Additional codecs for various formats
sudo apt install libavcodec-extra
```

### Graphics Drivers

#### NVIDIA Graphics

```bash path=null start=null
# Check for available NVIDIA drivers
ubuntu-drivers devices

# Install recommended NVIDIA driver
sudo ubuntu-drivers autoinstall

# Or install specific driver
sudo apt install nvidia-driver-XXX  # Replace XXX with version number

# Reboot after installation
sudo reboot
```

#### AMD Graphics

```bash path=null start=null
# AMD drivers are usually included in kernel
# For latest drivers:
sudo add-apt-repository ppa:oibaf/graphics-drivers
sudo apt update && sudo apt upgrade
```

### System Configuration

#### Enable Firewall

```bash path=null start=null
# Enable UFW firewall
sudo ufw enable

# Check firewall status
sudo ufw status
```

#### Configure Automatic Updates

```bash path=null start=null
# Install unattended upgrades
sudo apt install unattended-upgrades

# Configure automatic security updates
sudo dpkg-reconfigure unattended-upgrades
```

---

## 📁 Data Migration

### Accessing Windows Data

#### Mount Windows Partition

```bash path=null start=null
# List available partitions
sudo fdisk -l

# Create mount point
sudo mkdir /mnt/windows

# Mount Windows partition (replace /dev/sda2 with your Windows partition)
sudo mount -t ntfs-3g /dev/sda2 /mnt/windows

# Access Windows data
ls /mnt/windows/Users/[YourWindowsUsername]/
```

#### Copy Data to Ubuntu

```bash path=null start=null
# Copy Documents
cp -r /mnt/windows/Users/[WindowsUsername]/Documents/* ~/Documents/

# Copy Pictures
cp -r /mnt/windows/Users/[WindowsUsername]/Pictures/* ~/Pictures/

# Copy Music
cp -r /mnt/windows/Users/[WindowsUsername]/Music/* ~/Music/

# Copy Videos
cp -r /mnt/windows/Users/[WindowsUsername]/Videos/* ~/Videos/

# Copy Desktop files
cp -r /mnt/windows/Users/[WindowsUsername]/Desktop/* ~/Desktop/
```

### Browser Data Migration

#### Firefox

```bash path=null start=null
# Windows Firefox profile location:
# /mnt/windows/Users/[Username]/AppData/Roaming/Mozilla/Firefox/Profiles/

# Ubuntu Firefox profile location:
# ~/.mozilla/firefox/

# Copy profile data (bookmarks, passwords, etc.)
cp -r /mnt/windows/Users/[Username]/AppData/Roaming/Mozilla/Firefox/Profiles/* ~/.mozilla/firefox/
```

#### Chrome/Edge

```bash path=null start=null
# Export bookmarks from Windows browser as HTML
# Import bookmarks in Ubuntu browser via:
# Menu > Bookmarks > Import bookmarks and settings
```

---

## 🎯 Windows Software Alternatives

### Essential Applications

| Windows Software | Ubuntu Alternative | Installation Command |
|-------------------|-------------------|---------------------|
| **Microsoft Office** | LibreOffice | Pre-installed |
| **Adobe Photoshop** | GIMP | `sudo apt install gimp` |
| **Notepad++** | VS Code | `sudo snap install code --classic` |
| **WinRAR/7-Zip** | Archive Manager | Pre-installed |
| **Windows Media Player** | VLC | `sudo apt install vlc` |
| **Internet Explorer/Edge** | Firefox/Chrome | Pre-installed/Download |
| **Outlook** | Thunderbird | `sudo apt install thunderbird` |
| **Adobe Reader** | Document Viewer | Pre-installed |
| **Paint** | GIMP/Pinta | `sudo apt install pinta` |
| **Windows Defender** | ClamAV | `sudo apt install clamav` |

### Gaming on Ubuntu

```bash path=null start=null
# Steam for Linux gaming
sudo apt install steam

# Lutris for Windows games
sudo apt install lutris

# Wine for Windows applications
sudo apt install wine

# PlayOnLinux (Wine GUI)
sudo apt install playonlinux
```

### Professional Software

#### Video Editing

```bash path=null start=null
# DaVinci Resolve (professional)
# Download from BlackMagic website

# OpenShot (user-friendly)
sudo apt install openshot

# Kdenlive (advanced)
sudo apt install kdenlive
```

#### Audio Editing

```bash path=null start=null
# Audacity (similar to Audacity on Windows)
sudo apt install audacity

# Ardour (professional DAW)
sudo apt install ardour
```

#### 3D Modeling

```bash path=null start=null
# Blender (cross-platform)
sudo apt install blender

# FreeCAD (CAD software)
sudo apt install freecad
```

---

## 🛠️ Troubleshooting

### Boot Issues

#### GRUB Not Showing

```bash path=null start=null
# Boot from Ubuntu Live USB
sudo mount /dev/sda2 /mnt  # Replace with your Ubuntu partition
sudo mount --bind /dev /mnt/dev
sudo mount --bind /proc /mnt/proc
sudo mount --bind /sys /mnt/sys
sudo chroot /mnt

# Reinstall GRUB
grub-install /dev/sda  # Replace with your disk
update-grub

# Exit and reboot
exit
sudo reboot
```

#### Windows Boot Option Missing

```bash path=null start=null
# Update GRUB to detect Windows
sudo update-grub

# Install os-prober if needed
sudo apt install os-prober
sudo os-prober
sudo update-grub
```

### Hardware Issues

#### WiFi Not Working

```bash path=null start=null
# Check WiFi adapter
lspci | grep -i wireless
lsusb | grep -i wireless

# Install additional drivers
sudo ubuntu-drivers autoinstall

# For specific WiFi chipsets:
sudo apt install firmware-b43-installer      # Broadcom
sudo apt install firmware-iwlwifi            # Intel
```

#### Audio Not Working

```bash path=null start=null
# Check audio devices
aplay -l

# Restart audio service
sudo systemctl restart alsa-state
pulseaudio -k && pulseaudio --start

# Install additional audio packages
sudo apt install pavucontrol alsa-utils
```

#### Graphics Issues

```bash path=null start=null
# Switch to open-source drivers if proprietary drivers cause issues
sudo apt remove nvidia-* --purge
sudo apt install xserver-xorg-video-nouveau

# Or install different NVIDIA driver version
sudo apt install nvidia-driver-470  # Try different version
```

---

## 🚀 Tips for New Ubuntu Users

### Essential Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| `Ctrl+Alt+T` | Open terminal |
| `Super` | Open activities overview |
| `Alt+Tab` | Switch between applications |
| `Ctrl+Shift+C/V` | Copy/paste in terminal |
| `Super+L` | Lock screen |
| `Alt+F4` | Close application |

### Terminal Basics

```bash path=null start=null
# Navigate directories
cd /path/to/directory
cd ~        # Go to home directory
cd ..       # Go back one directory

# List files
ls          # Basic listing
ls -la      # Detailed listing with hidden files

# File operations
cp file1 file2      # Copy file
mv file1 file2      # Move/rename file
rm file1            # Delete file
mkdir newfolder     # Create directory
```

### Software Management

```bash path=null start=null
# Update system
sudo apt update && sudo apt upgrade

# Install software
sudo apt install package-name

# Remove software
sudo apt remove package-name

# Search for software
apt search keyword
```

---

## 📚 Learning Resources

### Ubuntu Documentation
- **Official Ubuntu Documentation**: https://help.ubuntu.com/
- **Ubuntu Community Help**: https://help.ubuntu.com/community/
- **Ubuntu Forums**: https://ubuntuforums.org/

### Learning Linux
- **Linux Command Line Tutorial**: Various online tutorials
- **Ubuntu Desktop Guide**: Built-in help system
- **YouTube Channels**: Dedicated to Ubuntu tutorials

### Community Support
- **Ask Ubuntu**: https://askubuntu.com/
- **Reddit**: r/Ubuntu, r/linux4noobs
- **Local Linux User Groups**: Check for groups in your area

---

## 🎯 Migration Checklist

### Pre-Installation
- [ ] Backup all important Windows data
- [ ] Create Windows recovery media
- [ ] Disable BitLocker encryption
- [ ] Disable Fast Startup
- [ ] Download Ubuntu ISO
- [ ] Create Ubuntu installation USB

### During Installation
- [ ] Test Ubuntu live environment
- [ ] Check hardware compatibility
- [ ] Choose installation type (single boot vs dual boot)
- [ ] Create user account with strong password
- [ ] Complete installation process

### Post-Installation
- [ ] Update system packages
- [ ] Install graphics drivers
- [ ] Configure firewall
- [ ] Install essential software
- [ ] Migrate data from Windows
- [ ] Set up automatic backups
- [ ] Configure browser and email
- [ ] Test all hardware functionality

### Learning Phase
- [ ] Learn basic terminal commands
- [ ] Explore Ubuntu desktop environment
- [ ] Install alternative software for Windows programs
- [ ] Join Ubuntu community forums
- [ ] Set up development environment (if needed)

---

## 🏆 Success Tips

### Gradual Transition Strategy

1. **Week 1-2**: Basic navigation and file management
2. **Week 3-4**: Installing software and system configuration
3. **Month 2**: Advanced features and customization
4. **Month 3+**: Contributing to community and advanced usage

### Common Pitfalls to Avoid

1. **Don't panic** if something doesn't work immediately
2. **Use Google/Ask Ubuntu** before making system changes
3. **Keep Windows backup** until fully comfortable with Ubuntu
4. **Start with LTS version** for stability
5. **Join communities** for support and learning

---

> 🔄 **Migration Tip**: Take your time with the transition. Keep your Windows backup until you're completely comfortable with Ubuntu. The Linux community is very helpful, so don't hesitate to ask questions!

**Next Steps**: Now that you've migrated to Ubuntu, explore our [Essential Commands](essential-commands.md) guide and [Productivity Tips](../productivity/) to get the most out of your new system!
