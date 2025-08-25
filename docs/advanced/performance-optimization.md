# ⚡ Ubuntu Performance Optimization

> **Maximize your Ubuntu system's speed and responsiveness with these proven optimization techniques**

## 📋 Table of Contents

- [🚀 Quick Performance Boosts](#-quick-performance-boosts)
- [💾 Memory Optimization](#-memory-optimization)
- [🖥️ CPU Optimization](#️-cpu-optimization)
- [💿 Storage Optimization](#-storage-optimization)
- [🎮 Graphics & Display](#-graphics--display)
- [🔧 System Services](#-system-services)
- [📊 Performance Monitoring](#-performance-monitoring)
- [⚙️ Advanced Tweaks](#️-advanced-tweaks)

---

## 🚀 Quick Performance Boosts

### 5-Minute Performance Setup

```bash path=null start=null
# Update system packages
sudo apt update && sudo apt upgrade -y

# Install preload for faster app launches
sudo apt install preload

# Enable zram for better memory management
sudo apt install zram-config

# Install performance monitoring tools
sudo apt install htop iotop nethogs

# Clean package cache
sudo apt autoclean && sudo apt autoremove
```

### Immediate Performance Checklist

- [ ] Remove unnecessary startup applications
- [ ] Enable preload for faster app loading
- [ ] Configure swap file appropriately
- [ ] Disable visual effects if using older hardware
- [ ] Clean up disk space

---

## 💾 Memory Optimization

### RAM Management

#### Check Memory Usage

```bash path=null start=null
# Check memory usage
free -h

# Detailed memory information
cat /proc/meminfo

# Show memory usage by process
ps aux --sort=-%mem | head -20

# Interactive memory monitoring
htop
```

#### Swap Configuration

```bash path=null start=null
# Check current swap usage
swapon --show
free -h

# Check swappiness value (default 60)
cat /proc/sys/vm/swappiness

# Optimize swappiness for desktop use (recommended: 10-20)
echo 'vm.swappiness=10' | sudo tee -a /etc/sysctl.conf

# For SSD users - reduce swappiness even more
echo 'vm.swappiness=1' | sudo tee -a /etc/sysctl.conf

# Apply changes immediately
sudo sysctl vm.swappiness=10
```

#### Create or Resize Swap File

```bash path=null start=null
# Check existing swap
sudo swapon --show

# Turn off existing swap (if resizing)
sudo swapoff -a

# Create new swap file (4GB example)
sudo fallocate -l 4G /swapfile

# Set proper permissions
sudo chmod 600 /swapfile

# Make it a swap file
sudo mkswap /swapfile

# Enable the swap file
sudo swapon /swapfile

# Make it permanent
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab

# Verify
sudo swapon --show
```

#### zRAM Setup

```bash path=null start=null
# Install zram-config
sudo apt install zram-config

# Check zram status
zramctl

# Configure zram size (edit configuration)
sudo nano /usr/bin/init-zram-swapping

# Restart zram service
sudo systemctl restart zram-config
```

### Memory Cleanup

```bash path=null start=null
# Clear page cache, dentries, and inodes
sudo sh -c 'echo 1 > /proc/sys/vm/drop_caches'

# Clear all caches (more aggressive)
sudo sh -c 'echo 3 > /proc/sys/vm/drop_caches'

# Clear swap (if needed)
sudo swapoff -a && sudo swapon -a

# Clean package manager cache
sudo apt clean
sudo apt autoclean
sudo apt autoremove
```

---

## 🖥️ CPU Optimization

### CPU Governor Settings

```bash path=null start=null
# Install CPU frequency utilities
sudo apt install cpufrequtils

# Check current CPU governor
cat /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor

# Available governors
cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_available_governors

# Set performance governor (for maximum performance)
echo performance | sudo tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor

# Set ondemand governor (balanced, recommended for laptops)
echo ondemand | sudo tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor

# Make permanent
echo 'GOVERNOR="ondemand"' | sudo tee -a /etc/default/cpufrequtils
```

### CPU Performance Tuning

```bash path=null start=null
# Check CPU information
lscpu
cat /proc/cpuinfo

# Monitor CPU usage by process
top -o %CPU
htop

# Install and use s-tui for CPU monitoring
sudo apt install s-tui stress
s-tui

# Stress test CPU (be careful!)
stress --cpu 4 --timeout 60s
```

### Process Priority Management

```bash path=null start=null
# Set process priority (nice values: -20 to 19)
# Lower values = higher priority

# Start process with specific priority
nice -n -10 your_important_command

# Change priority of running process
sudo renice -10 -p PID

# Use ionice for I/O priority
sudo ionice -c 1 -n 0 -p PID  # Real-time I/O class
```

---

## 💿 Storage Optimization

### SSD Optimization

#### Enable TRIM

```bash path=null start=null
# Check if SSD supports TRIM
sudo hdparm -I /dev/sda | grep TRIM

# Enable TRIM support
sudo systemctl enable fstrim.timer
sudo systemctl start fstrim.timer

# Manual TRIM (run weekly)
sudo fstrim -av

# Check TRIM status
sudo systemctl status fstrim.timer
```

#### SSD Mount Optimizations

```bash path=null start=null
# Edit fstab for SSD optimizations
sudo nano /etc/fstab

# Add noatime,discard options to SSD partitions
# Example:
# UUID=xxx / ext4 defaults,noatime,discard 0 1

# Apply changes
sudo mount -o remount /
```

### Disk Performance

#### I/O Scheduler Optimization

```bash path=null start=null
# Check current I/O scheduler
cat /sys/block/sda/queue/scheduler

# For SSDs, use mq-deadline or kyber
echo mq-deadline | sudo tee /sys/block/sda/queue/scheduler

# For HDDs, use bfq or cfq
echo bfq | sudo tee /sys/block/sda/queue/scheduler

# Make permanent
echo 'GRUB_CMDLINE_LINUX_DEFAULT="elevator=mq-deadline"' | sudo tee -a /etc/default/grub
sudo update-grub
```

#### Filesystem Tuning

```bash path=null start=null
# Check filesystem information
df -Th
sudo tune2fs -l /dev/sda1

# Optimize ext4 filesystem
sudo tune2fs -o journal_data_writeback /dev/sda1

# Set reserved blocks percentage (default 5%, reduce to 1% for large drives)
sudo tune2fs -m 1 /dev/sda1

# Enable dir_index for faster directory access
sudo tune2fs -O dir_index /dev/sda1
```

### Disk Cleanup

```bash path=null start=null
# Check disk usage
df -h
du -sh /* 2>/dev/null | sort -hr

# Clean package cache
sudo apt clean
sudo apt autoclean

# Remove orphaned packages
sudo apt autoremove --purge

# Clean snap packages
sudo snap list --all | awk '/disabled/{print $1, $3}' | while read snapname revision; do sudo snap remove "$snapname" --revision="$revision"; done

# Clean system logs
sudo journalctl --disk-usage
sudo journalctl --vacuum-time=7d

# Clean thumbnail cache
rm -rf ~/.cache/thumbnails/*

# Find and remove large files
find / -size +100M -type f 2>/dev/null | head -20
```

---

## 🎮 Graphics & Display

### Graphics Driver Optimization

```bash path=null start=null
# Check graphics information
lspci | grep VGA
glxinfo | grep "OpenGL renderer"

# Install additional drivers
sudo ubuntu-drivers autoinstall

# For NVIDIA users
sudo apt install nvidia-driver-$(ubuntu-drivers devices | grep nvidia | head -1 | cut -d' ' -f3 | cut -d'-' -f3)

# For AMD users
sudo apt install mesa-vulkan-drivers vulkan-utils
```

### Display Settings

```bash path=null start=null
# Check display information
xrandr

# Set optimal refresh rate
xrandr --output HDMI-1 --mode 1920x1080 --rate 144

# Disable visual effects (for older hardware)
gsettings set org.gnome.desktop.interface enable-animations false
gsettings set org.gnome.shell.extensions.dash-to-dock animate-show-apps false
```

### Compositor Settings

```bash path=null start=null
# For older hardware, consider disabling compositor
# This will reduce visual effects but improve performance

# Install compton (lightweight compositor alternative)
sudo apt install compton

# Create compton config
mkdir -p ~/.config/compton
cat > ~/.config/compton/compton.conf << EOF
# Performance-focused compton config
backend = "glx";
glx-no-stencil = true;
glx-copy-from-front = false;
glx-swap-method = "undefined";
shadow = false;
fading = false;
EOF
```

---

## 🔧 System Services

### Disable Unnecessary Services

```bash path=null start=null
# List all services
systemctl list-units --type=service --state=running

# Disable services you don't need (examples)
sudo systemctl disable bluetooth.service      # If no Bluetooth devices
sudo systemctl disable cups.service          # If no printer
sudo systemctl disable whoopsie.service      # Error reporting
sudo systemctl disable kerneloops.service    # Kernel error reporting
sudo systemctl disable apport.service        # Crash reporting

# Check what services are enabled at boot
systemctl list-unit-files --type=service --state=enabled

# Disable specific services (be careful!)
# sudo systemctl disable service_name.service
```

### Startup Applications

```bash path=null start=null
# List startup applications
ls /etc/xdg/autostart/
ls ~/.config/autostart/

# Use GUI to manage startup applications
gnome-session-properties

# Or manually disable by adding Hidden=true to .desktop files
echo "Hidden=true" | tee -a ~/.config/autostart/unwanted_app.desktop
```

### Journal and Logging

```bash path=null start=null
# Limit systemd journal size
sudo nano /etc/systemd/journald.conf

# Add these settings:
# SystemMaxUse=100M
# SystemMaxFileSize=10M
# MaxRetentionSec=1week

# Restart journald
sudo systemctl restart systemd-journald

# Clean old logs
sudo journalctl --vacuum-size=50M
sudo journalctl --vacuum-time=2weeks
```

---

## 📊 Performance Monitoring

### Essential Monitoring Tools

```bash path=null start=null
# Install monitoring tools
sudo apt install htop iotop nethogs nmon sysstat

# CPU and memory monitoring
htop                    # Interactive process viewer
top                     # Basic process viewer
nmon                    # Comprehensive system monitor

# Disk I/O monitoring  
iotop                   # I/O usage by process
iostat 1                # I/O statistics

# Network monitoring
nethogs                 # Network usage by process
iftop                   # Network bandwidth usage
```

### System Performance Analysis

```bash path=null start=null
# Generate system activity report
sar -u 1 10            # CPU usage for 10 seconds
sar -r 1 10            # Memory usage
sar -d 1 10            # Disk activity

# Process monitoring
ps aux --sort=-%cpu | head -10     # Top CPU processes
ps aux --sort=-%mem | head -10     # Top memory processes

# System uptime and load
uptime
w

# Detailed system information
inxi -Fx
neofetch
```

### Benchmarking

```bash path=null start=null
# Install benchmarking tools
sudo apt install sysbench hdparm

# CPU benchmark
sysbench cpu --threads=4 run

# Memory benchmark
sysbench memory run

# Disk benchmark
hdparm -Tt /dev/sda
dd if=/dev/zero of=testfile bs=1G count=1 oflag=direct

# Network benchmark (between two machines)
# On server: iperf3 -s
# On client: iperf3 -c server_ip
```

---

## ⚙️ Advanced Tweaks

### Kernel Parameters

```bash path=null start=null
# Edit kernel parameters
sudo nano /etc/sysctl.conf

# Add performance optimizations:
# Increase file descriptor limits
fs.file-max = 65536

# Network performance
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216
net.ipv4.tcp_rmem = 4096 87380 16777216
net.ipv4.tcp_wmem = 4096 65536 16777216

# Virtual memory tuning
vm.dirty_ratio = 15
vm.dirty_background_ratio = 5
vm.vfs_cache_pressure = 50

# Apply changes
sudo sysctl -p
```

### GRUB Optimizations

```bash path=null start=null
# Edit GRUB configuration
sudo nano /etc/default/grub

# Add performance kernel parameters
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=mq-deadline"

# For systems with ample RAM, add:
# GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=mq-deadline mitigations=off"

# Reduce GRUB timeout
GRUB_TIMEOUT=2

# Update GRUB
sudo update-grub
```

### Profile-based Optimization

```bash path=null start=null
# Install TLP for laptop power management
sudo apt install tlp tlp-rdw

# Start TLP
sudo tlp start

# Check TLP status
sudo tlp-stat

# Install auto-cpufreq for automatic CPU scaling
git clone https://github.com/AdnanHodzic/auto-cpufreq.git
cd auto-cpufreq && sudo ./auto-cpufreq-installer

# Enable auto-cpufreq
sudo auto-cpufreq --install
```

### Gaming Optimizations

```bash path=null start=null
# Install GameMode for game performance
sudo apt install gamemode

# Use GameMode with games
gamemoderun your_game

# Install Lutris for game management
sudo apt install lutris

# For Steam games, add to launch options:
# gamemoderun %command%

# Optimize graphics for gaming
sudo apt install vulkan-utils
vulkaninfo | grep "deviceName"
```

---

## 🎯 Hardware-Specific Optimizations

### Laptop Optimizations

```bash path=null start=null
# Install laptop-specific tools
sudo apt install laptop-mode-tools powertop

# Run powertop for power analysis
sudo powertop --calibrate
sudo powertop --auto-tune

# Enable laptop-mode
sudo laptop_mode

# Configure CPU scaling for battery
echo powersave | sudo tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
```

### Multi-core Optimizations

```bash path=null start=null
# Check CPU cores
nproc

# Set CPU affinity for processes
taskset -c 0-3 your_process

# Use all cores for compilation
export MAKEFLAGS="-j$(nproc)"

# Configure irqbalance for multi-core systems
sudo systemctl enable irqbalance
```

---

## 🔍 Performance Troubleshooting

### Identify Performance Bottlenecks

```bash path=null start=null
# Check system load
uptime
cat /proc/loadavg

# Identify high CPU processes
ps aux --sort=-%cpu | head -10

# Find processes using most memory
ps aux --sort=-%mem | head -10

# Check disk I/O
iotop -o

# Monitor in real-time
watch -n 1 'ps aux --sort=-%cpu | head -20'
```

### Common Performance Issues

#### High CPU Usage
```bash path=null start=null
# Find CPU-intensive processes
top -o %CPU

# Check for CPU throttling
cat /proc/cpuinfo | grep MHz

# Monitor CPU temperature
sensors
```

#### Memory Issues
```bash path=null start=null
# Check for memory leaks
valgrind --tool=memcheck your_program

# Monitor swap usage
watch -n 1 free -m

# Find memory-hungry processes
pmap -x PID
```

#### Disk I/O Problems
```bash path=null start=null
# Check disk usage
df -h
du -sh /*

# Monitor disk I/O
iostat -x 1

# Check for disk errors
sudo dmesg | grep -i error
```

---

## 📈 Performance Profiles

### Create Performance Profiles

```bash path=null start=null
# Create performance script
cat > ~/performance-mode.sh << 'EOF'
#!/bin/bash
# Performance mode script

# Set CPU governor to performance
echo performance | sudo tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor

# Disable CPU power saving
echo off | sudo tee /sys/devices/system/cpu/smt/control 2>/dev/null || true

# Set I/O scheduler to deadline
echo mq-deadline | sudo tee /sys/block/*/queue/scheduler

# Reduce swappiness
echo 1 | sudo tee /proc/sys/vm/swappiness

echo "Performance mode enabled"
EOF

# Make executable
chmod +x ~/performance-mode.sh

# Create power-saving script
cat > ~/powersave-mode.sh << 'EOF'
#!/bin/bash
# Power saving mode script

# Set CPU governor to powersave
echo powersave | sudo tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor

# Enable CPU power saving
echo on | sudo tee /sys/devices/system/cpu/smt/control 2>/dev/null || true

# Set I/O scheduler to bfq
echo bfq | sudo tee /sys/block/*/queue/scheduler

# Increase swappiness
echo 60 | sudo tee /proc/sys/vm/swappiness

echo "Power saving mode enabled"
EOF

chmod +x ~/powersave-mode.sh
```

---

## 🏆 Performance Best Practices

### Daily Performance Habits

1. **Monitor Resource Usage**
   ```bash path=null start=null
   htop
   ```

2. **Clean System Regularly**
   ```bash path=null start=null
   sudo apt autoremove && sudo apt autoclean
   ```

3. **Check Disk Space**
   ```bash path=null start=null
   df -h
   ```

### Weekly Performance Tasks

- [ ] Run disk cleanup
- [ ] Check for memory leaks in long-running applications
- [ ] Update system packages
- [ ] Review startup applications
- [ ] Monitor temperature and fan speeds

### Monthly Performance Tasks

- [ ] Full system benchmark
- [ ] Review and optimize service list
- [ ] Check for driver updates
- [ ] Analyze system logs for errors
- [ ] Consider hardware upgrades if needed

---

## 🎚️ Performance Tuning by Use Case

### Development Workstation
```bash path=null start=null
# Optimize for compilation and development
export MAKEFLAGS="-j$(nproc)"
echo performance | sudo tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
echo 1 | sudo tee /proc/sys/vm/swappiness
```

### Media Production
```bash path=null start=null
# Optimize for video/audio editing
echo mq-deadline | sudo tee /sys/block/*/queue/scheduler
echo 50 | sudo tee /proc/sys/vm/vfs_cache_pressure
ulimit -n 65536
```

### Gaming System
```bash path=null start=null
# Install gamemode and configure
sudo apt install gamemode
echo performance | sudo tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
sudo sysctl kernel.sched_autogroup_enabled=0
```

### Server/Headless System
```bash path=null start=null
# Disable unnecessary GUI services
sudo systemctl set-default multi-user.target
sudo systemctl disable gdm3.service
echo bfq | sudo tee /sys/block/*/queue/scheduler
```

---

> ⚡ **Performance Tip**: Start with the Quick Performance Boosts and gradually implement other optimizations. Monitor system behavior after each change to ensure stability.

**Next Steps**: After optimizing performance, secure your speedy system with our [Security & Privacy Guide](security-privacy.md) or troubleshoot network issues with our [Networking Guide](../troubleshooting/networking.md).
