# 🎨 System Customization & Productivity

> **Transform your Ubuntu desktop into a productivity powerhouse with these essential customizations**

## 📋 Table of Contents

- [📋 Clipboard Management](#-clipboard-management)
- [📊 System Monitoring](#-system-monitoring)
- [📷 Screen Capture & OCR](#-screen-capture--ocr)
- [⚡ Performance Optimization](#-performance-optimization)
- [🔧 GNOME Extensions](#-gnome-extensions)
- [🔋 Battery Management](#-battery-management)
- [💿 Bootable Media Creation](#-bootable-media-creation)
- [📹 Camera & Media](#-camera--media)
- [💡 Brightness Control](#-brightness-control)

---

## 📋 Clipboard Management

### Setting up Clipboard History with Clipboard Indicator

Never lose copied text again! This extension provides clipboard history functionality similar to Windows or macOS.

#### 📥 Installation Steps

```bash path=null start=null
# Step 1: Install Extension Manager
# Go to App Center > search "Extension Manager" > install it
```

#### 🔧 Setup Process

1. **Open Extension Manager**
   - Launch Extension Manager from your applications menu

2. **Find Clipboard Indicator**
   - Go to the "Browse" tab in Extension Manager
   - Search for "Clipboard Indicator"
   - Look for the extension by developer **"Tudmotu"**

3. **Install and Enable**
   - Click Install on the Clipboard Indicator extension
   - The extension should enable automatically

4. **Troubleshooting** (if extension doesn't appear):
   ```bash path=null start=null
   # Install GNOME Tweaks for manual extension management
   sudo apt install gnome-tweaks
   
   # Launch GNOME Tweaks
   gnome-tweaks
   ```

#### ⚡ Usage Tips

- **Access History**: Click the clipboard icon in the top bar
- **Clear History**: Right-click the icon and select "Clear History"
- **Pin Items**: Pin frequently used clips to keep them permanently
- **Keyboard Shortcut**: Set up custom shortcuts in Settings > Keyboard

---

## 📊 System Monitoring

### Real-time System Monitor Extension

Keep an eye on your system's performance directly in the top bar with CPU, memory, and network usage indicators.

#### 📥 Installation

```bash path=null start=null
# Install required dependencies
sudo apt-get update -y
sudo apt-get install -y gir1.2-gtop-2.0

# Reboot to ensure dependencies are loaded
sudo reboot
```

#### 🔧 Setup

1. **Open Extension Manager**
2. **Search for 'System Monitor'**
   - Select the option with developer **'fmuellner'**
   - This is the most reliable and feature-rich system monitor extension

#### 🎛️ Configuration Options

- **CPU Usage**: Real-time CPU percentage
- **Memory Usage**: RAM and swap usage
- **Network Activity**: Upload/download speeds
- **Disk I/O**: Read/write activity
- **Temperature**: CPU temperature monitoring (if supported)

#### 💡 Pro Tips

- Click on any metric to open detailed system information
- Customize which metrics to display in the extension settings
- Set up alerts for high CPU or memory usage

---

## 📷 Screen Capture & OCR

### NormCap - Extract Text from Screen

NormCap is a powerful OCR tool that can extract text from any part of your screen - perfect for copying text from images, PDFs, or videos.

#### 📥 Download & Installation

1. **Download NormCap**
   ```bash path=null start=null
   # Visit: https://dynobo.github.io/normcap/
   # Download the latest .AppImage file
   ```

2. **Create Application Directory**
   ```bash path=null start=null
   # Create a directory for AppImage files (avoid naming it 'bin')
   mkdir -p ~/Applications
   
   # Move the AppImage to this directory
   mv ~/Downloads/NormCap-*.AppImage ~/Applications/
   ```

3. **Make Executable**
   ```bash path=null start=null
   # Navigate to Applications directory
   cd ~/Applications
   
   # Make AppImage executable (replace with actual filename)
   chmod +x NormCap-*.AppImage
   
   # Install FUSE (required for AppImage files)
   sudo apt install libfuse2
   ```

4. **Test the Application**
   ```bash path=null start=null
   # Test run the application
   ./NormCap-*.AppImage
   ```

#### 🔧 Create Launch Script

Create a convenient launch script:

```bash path=null start=null
# Create launch script
touch ~/Applications/normcap.sh
nano ~/Applications/normcap.sh
```

Add this content to the script:
```bash path=/home/username/Applications/normcap.sh start=1
#!/bin/bash
/home/$(whoami)/Applications/NormCap-0.5.8-x86_64.AppImage
```

Make the script executable:
```bash path=null start=null
chmod +x ~/Applications/normcap.sh
```

#### ⌨️ Set up Keyboard Shortcut

1. **Open Settings > Keyboard > View and Customize Shortcuts**
2. **Go to Custom Shortcuts**
3. **Click the + button to add new shortcut**
4. **Configure:**
   - **Name**: NormCap OCR
   - **Command**: `/home/$(whoami)/Applications/normcap.sh`
   - **Shortcut**: Choose your preferred key combination (e.g., `Ctrl+Shift+O`)

#### 🎯 Usage Tips

- **Select Area**: Click and drag to select the area containing text
- **Multiple Languages**: NormCap supports multiple languages automatically
- **Copy to Clipboard**: Extracted text is automatically copied
- **Save to File**: Option to save extracted text directly to a file

---

## ⚡ Performance Optimization

### Preload - Improve Application Launch Speed

Preload analyzes your usage patterns and preloads frequently used applications into memory for faster startup times.

```bash path=null start=null
# Install preload
sudo apt install preload

# Preload starts automatically and learns your usage patterns
# No additional configuration needed - it works in the background
```

#### 📈 What Preload Does

- **Learning Phase**: Monitors which applications you use most
- **Preloading**: Loads frequently used app components into RAM
- **Adaptive**: Adjusts based on your changing usage patterns
- **Background Operation**: Works silently without user intervention

#### 💡 Performance Tips

- **Give it Time**: Preload needs 2-3 weeks to learn your patterns effectively
- **Sufficient RAM**: Most effective on systems with 4GB+ RAM
- **SSD Benefit**: Less noticeable on SSDs but still provides some improvement

---

## 🔧 GNOME Extensions

### Extension Management

GNOME Extensions provide powerful customization options for your desktop environment.

#### 📥 Install Extensions Application

```bash path=null start=null
# Install GNOME Shell Extensions
sudo apt update
sudo apt install gnome-shell-extensions

# Install Extensions app for easier management
sudo apt install gnome-shell-extension-prefs

# Reboot recommended after installation
sudo reboot
```

#### 🎯 Essential Extensions to Try

1. **Dash to Dock** - macOS-like dock functionality
2. **User Themes** - Custom theme support
3. **Workspace Indicator** - Quick workspace switching
4. **OpenWeather** - Weather information in top bar
5. **Night Theme Switcher** - Automatic dark/light theme switching

#### 🔧 Managing Extensions

- **Extensions App**: Use the dedicated Extensions application
- **GNOME Tweaks**: Alternative interface for extension management
- **Command Line**: `gnome-extensions list` to see installed extensions

---

## 🔋 Battery Management

### Limit Battery Charging (Laptop Users)

Extend your laptop battery lifespan by limiting maximum charge to 80%.

> ⚠️ **Note**: This feature may not work on all laptop models. Test carefully and revert if issues occur.

#### 🔍 Check Battery Information

```bash path=null start=null
# List power supply devices
ls /sys/class/power_supply/

# Check battery capabilities
ls /sys/class/power_supply/BAT0/

# Look for charge control files
ls /sys/class/power_supply/BAT0/ | grep -i charge
```

#### 🛠️ Create Battery Service

```bash path=null start=null
# Create systemd service file
sudo nano /etc/systemd/system/battery-charge-end-threshold.service
```

Add this configuration:
```ini path=/etc/systemd/system/battery-charge-end-threshold.service start=1
[Unit]
Description=Set Battery Charge End Threshold
After=multi-user.target

[Service]
Type=oneshot
ExecStart=/bin/sh -c 'echo 80 > /sys/class/power_supply/BAT0/charge_control_end_threshold'
RemainAfterExit=true

[Install]
WantedBy=multi-user.target
```

#### ⚡ Enable the Service

```bash path=null start=null
# Enable the service to start at boot
sudo systemctl enable battery-charge-end-threshold.service

# Start the service immediately
sudo systemctl start battery-charge-end-threshold.service

# Check service status
sudo systemctl status battery-charge-end-threshold.service
```

#### 🔧 Verify Settings

```bash path=null start=null
# Check current charge threshold
cat /sys/class/power_supply/BAT0/charge_control_end_threshold

# Should output: 80
```

---

## 💿 Bootable Media Creation

### Creating Ubuntu Bootable Drives

#### 🛠️ Built-in Solution: Startup Disk Creator

```bash path=null start=null
# Install Startup Disk Creator
sudo apt update
sudo apt install usb-creator-gtk
```

**Features:**
- **Ubuntu Optimized**: Works best with Ubuntu ISO files
- **Simple Interface**: User-friendly GUI
- **Persistence**: Option to save changes on the USB drive

**Limitation**: Sometimes doesn't recognize ISO files - try renaming `.iso` to `.img`

#### 🚀 Universal Solution: Ventoy

For creating bootable drives with multiple operating systems:

1. **Download Ventoy**: Visit https://github.com/ventoy/Ventoy/releases
2. **Installation**: Simple drag-and-drop ISO files to the USB drive
3. **Multi-boot**: Boot multiple ISOs from a single USB drive
4. **Cross-platform**: Works with Windows, Linux, and other OS ISOs

#### 💡 Pro Tips

- **USB 3.0**: Use USB 3.0 drives for faster boot and installation
- **Size Matters**: Use at least 8GB USB drives for modern OS ISOs
- **Backup**: Always backup important data from USB drives before creating bootable media

---

## 📹 Camera & Media

### Camera Application Setup

Access your camera for video calls, streaming, and photography.

```bash path=null start=null
# Install guvcview camera application
sudo apt update
sudo apt-get install guvcview

# Reboot recommended for proper camera detection
sudo reboot
```

#### 🎥 Usage

```bash path=null start=null
# Launch from command line
guvcview

# Or find "guvcview" in your applications menu
```

#### ✨ Features

- **Video Recording**: Record videos in various formats
- **Photo Capture**: Take still photos
- **Settings Control**: Adjust brightness, contrast, resolution
- **Multiple Cameras**: Switch between cameras if multiple are available

#### 🔧 Troubleshooting Camera Issues

```bash path=null start=null
# Check if camera is detected
lsusb | grep -i camera

# Check video devices
ls /dev/video*

# Test camera with simple command
ffplay /dev/video0
```

---

## 💡 Brightness Control

### Keyboard Shortcuts for Brightness

Set up custom keyboard shortcuts for precise brightness control.

#### 🔧 Method 1: Using brightnessctl

```bash path=null start=null
# Install brightnessctl
sudo apt install brightnessctl

# Test brightness control
brightnessctl set +10%  # Increase by 10%
brightnessctl set 10%-  # Decrease by 10%
```

#### 🔑 Configure Sudo Access

To use brightness controls without password prompts:

```bash path=null start=null
# Edit sudoers file
sudo visudo

# Add this line (replace 'username' with your actual username):
# username ALL=(ALL) NOPASSWD: /usr/bin/brightnessctl
```

#### ⌨️ Set up Keyboard Shortcuts

1. **Settings > Keyboard > View and Customize Shortcuts > Custom Shortcuts**
2. **Add shortcuts:**

**Increase Brightness:**
- **Name**: Increase Brightness
- **Command**: `sudo brightnessctl set +10%`
- **Shortcut**: `Fn+F6` or custom combination

**Decrease Brightness:**
- **Name**: Decrease Brightness  
- **Command**: `sudo brightnessctl set 10%-`
- **Shortcut**: `Fn+F5` or custom combination

#### 🔧 Method 2: Using gdbus (Alternative)

If brightnessctl doesn't work:

```bash path=null start=null
# Increase brightness
gdbus call --session --dest org.gnome.SettingsDaemon.Power --object-path /org/gnome/SettingsDaemon/Power --method org.gnome.SettingsDaemon.Power.Screen.StepUp

# Decrease brightness
gdbus call --session --dest org.gnome.SettingsDaemon.Power --object-path /org/gnome/SettingsDaemon/Power --method org.gnome.SettingsDaemon.Power.Screen.StepDown
```

Use these commands in your custom keyboard shortcuts if the first method doesn't work.

---

## 🔍 System Information

### Check Computer Serial Number

Useful for warranty claims, asset management, or system identification:

```bash path=null start=null
# Get system serial number
sudo dmidecode -s system-serial-number

# Get more detailed system information
sudo dmidecode -s system-manufacturer
sudo dmidecode -s system-product-name
sudo dmidecode -s system-version
```

---

> 💡 **Pro Tip**: After implementing these customizations, create a backup of your settings so you can quickly restore them on a fresh installation!

**Next Steps**: Explore more [Productivity Tips](workflow-optimization.md) or check out [Advanced Customizations](../advanced/advanced-customization.md).
