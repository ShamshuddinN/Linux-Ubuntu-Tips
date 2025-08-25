# 🌐 Network Troubleshooting Guide

> **Diagnose and fix network connectivity issues on Ubuntu with systematic troubleshooting approaches**

## 📋 Table of Contents

- [🚀 Quick Network Diagnosis](#-quick-network-diagnosis)
- [🔧 WiFi Issues](#-wifi-issues)
- [🌐 Internet Connection Problems](#-internet-connection-problems)
- [🖥️ Network Configuration](#️-network-configuration)
- [🔒 VPN Troubleshooting](#-vpn-troubleshooting)
- [📡 Bluetooth Connectivity](#-bluetooth-connectivity)
- [🛠️ Advanced Network Tools](#️-advanced-network-tools)
- [🔍 Network Monitoring](#-network-monitoring)

---

## 🚀 Quick Network Diagnosis

### Essential Network Check Commands

```bash path=null start=null
# Check network interface status
ip addr show

# Test internet connectivity
ping -c 4 8.8.8.8

# Check DNS resolution
nslookup google.com

# View network routes
ip route show

# Check active connections
ss -tuln
```

### Network Status Overview

```bash path=null start=null
# Comprehensive network status
nmcli general status
nmcli connection show

# Network manager status
systemctl status NetworkManager

# Check if network interfaces are up
ip link show

# Display network statistics
cat /proc/net/dev
```

---

## 🔧 WiFi Issues

### WiFi Not Detected

#### Check WiFi Hardware

```bash path=null start=null
# Check if WiFi adapter is detected
lspci | grep -i wireless
lsusb | grep -i wireless

# Check network interfaces
ip addr show
iwconfig

# Check if WiFi is enabled
rfkill list all

# Enable WiFi if blocked
sudo rfkill unblock wifi
```

#### Install Missing WiFi Drivers

```bash path=null start=null
# Update system first
sudo apt update && sudo apt upgrade -y

# Install additional drivers
sudo ubuntu-drivers autoinstall

# For specific WiFi chipsets:
# Realtek
sudo apt install rtl8812au-dkms

# Intel
sudo apt install firmware-iwlwifi

# Broadcom
sudo apt install bcmwl-kernel-source

# Restart NetworkManager
sudo systemctl restart NetworkManager
```

### WiFi Connection Problems

#### Scan for Available Networks

```bash path=null start=null
# Scan for WiFi networks
sudo iwlist scan | grep ESSID

# Using NetworkManager CLI
nmcli device wifi list

# Refresh available networks
nmcli device wifi rescan
```

#### Connect to WiFi Network

```bash path=null start=null
# Connect to open network
nmcli device wifi connect "NetworkName"

# Connect to WPA/WPA2 network
nmcli device wifi connect "NetworkName" password "YourPassword"

# Connect to hidden network
nmcli connection add type wifi con-name "MyConnection" ifname wlan0 ssid "HiddenNetwork"
nmcli connection modify "MyConnection" wifi-sec.key-mgmt wpa-psk
nmcli connection modify "MyConnection" wifi-sec.psk "YourPassword"
nmcli connection up "MyConnection"
```

### WiFi Keeps Disconnecting

#### Power Management Issues

```bash path=null start=null
# Check power management status
iwconfig wlan0 | grep "Power Management"

# Disable power management temporarily
sudo iwconfig wlan0 power off

# Make permanent - create configuration file
echo 'options iwlwifi power_save=0' | sudo tee /etc/modprobe.d/iwlwifi.conf

# For other adapters, find module name first
lsmod | grep -E "(iwl|rt|ath)"
```

#### DNS and DHCP Issues

```bash path=null start=null
# Release and renew IP address
sudo dhclient -r wlan0
sudo dhclient wlan0

# Flush DNS cache
sudo systemctl restart systemd-resolved

# Check DNS configuration
systemd-resolve --status

# Set manual DNS servers
nmcli connection modify "YourWiFi" ipv4.dns "8.8.8.8,1.1.1.1"
nmcli connection up "YourWiFi"
```

---

## 🌐 Internet Connection Problems

### No Internet Access (Local Network OK)

#### DNS Troubleshooting

```bash path=null start=null
# Test connectivity to IP address (bypassing DNS)
ping -c 4 8.8.8.8

# Test DNS resolution
nslookup google.com
dig google.com

# Check current DNS servers
systemd-resolve --status | grep "DNS Servers"

# Flush DNS cache
sudo systemd-resolve --flush-caches

# Test with different DNS servers
nslookup google.com 8.8.8.8
```

#### Change DNS Servers

```bash path=null start=null
# Using NetworkManager (temporary)
nmcli connection modify "YourConnection" ipv4.dns "8.8.8.8,1.1.1.1"
nmcli connection modify "YourConnection" ipv6.dns "2001:4860:4860::8888,2001:4860:4860::8844"
nmcli connection up "YourConnection"

# Edit resolved configuration (permanent)
sudo nano /etc/systemd/resolved.conf

# Add these lines:
# DNS=8.8.8.8 1.1.1.1
# FallbackDNS=8.8.4.4 1.0.0.1

# Restart DNS service
sudo systemctl restart systemd-resolved
```

### Slow Internet Speed

#### Network Speed Testing

```bash path=null start=null
# Install speedtest CLI
sudo apt install speedtest-cli

# Run speed test
speedtest-cli

# More detailed test
speedtest-cli --simple

# Test with specific server
speedtest-cli --list | head -20
speedtest-cli --server SERVERID
```

#### Optimize Network Settings

```bash path=null start=null
# Check current MTU
ip link show

# Find optimal MTU
ping -M do -s 1464 8.8.8.8

# Set MTU if needed
sudo ip link set dev eth0 mtu 1500

# Make permanent in NetworkManager
nmcli connection modify "YourConnection" ethernet.mtu 1500
```

---

## 🖥️ Network Configuration

### Static IP Configuration

#### Using NetworkManager

```bash path=null start=null
# Show current connections
nmcli connection show

# Configure static IP
nmcli connection modify "YourConnection" ipv4.addresses 192.168.1.100/24
nmcli connection modify "YourConnection" ipv4.gateway 192.168.1.1
nmcli connection modify "YourConnection" ipv4.dns "8.8.8.8,1.1.1.1"
nmcli connection modify "YourConnection" ipv4.method manual

# Apply changes
nmcli connection up "YourConnection"
```

#### Using Netplan (Ubuntu 18.04+)

```bash path=null start=null
# Edit netplan configuration
sudo nano /etc/netplan/01-netcfg.yaml

# Example static IP configuration:
```

```yaml path=/etc/netplan/01-netcfg.yaml start=null
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: no
      addresses:
        - 192.168.1.100/24
      gateway4: 192.168.1.1
      nameservers:
          addresses: [8.8.8.8, 1.1.1.1]
```

```bash path=null start=null
# Apply netplan configuration
sudo netplan apply

# Test configuration
sudo netplan try
```

### Network Interface Management

```bash path=null start=null
# List all network interfaces
ip link show

# Bring interface up/down
sudo ip link set eth0 up
sudo ip link set eth0 down

# Add IP address to interface
sudo ip addr add 192.168.1.100/24 dev eth0

# Remove IP address
sudo ip addr del 192.168.1.100/24 dev eth0

# View interface statistics
ip -s link show eth0
```

---

## 🔒 VPN Troubleshooting

### OpenVPN Issues

#### Installation and Setup

```bash path=null start=null
# Install OpenVPN
sudo apt install openvpn

# Import VPN configuration
sudo cp your-vpn-config.ovpn /etc/openvpn/client.conf

# Start VPN connection
sudo openvpn --config /etc/openvpn/client.conf

# Or as a service
sudo systemctl start openvpn@client
sudo systemctl enable openvpn@client
```

#### VPN Connection Problems

```bash path=null start=null
# Check VPN status
systemctl status openvpn@client

# View VPN logs
sudo journalctl -u openvpn@client -f

# Test VPN connection
curl ifconfig.me    # Check public IP before VPN
# Connect VPN
curl ifconfig.me    # Check public IP after VPN

# Common VPN fixes
# 1. DNS issues
echo 'script-security 2' | sudo tee -a /etc/openvpn/client.conf
echo 'up /etc/openvpn/update-resolv-conf' | sudo tee -a /etc/openvpn/client.conf
echo 'down /etc/openvpn/update-resolv-conf' | sudo tee -a /etc/openvpn/client.conf
```

### WireGuard VPN

```bash path=null start=null
# Install WireGuard
sudo apt install wireguard

# Generate keys
wg genkey | tee privatekey | wg pubkey > publickey

# Create configuration
sudo nano /etc/wireguard/wg0.conf

# Example configuration:
# [Interface]
# PrivateKey = YOUR_PRIVATE_KEY
# Address = 10.0.0.2/32
# DNS = 8.8.8.8
# 
# [Peer]
# PublicKey = SERVER_PUBLIC_KEY
# Endpoint = SERVER_IP:51820
# AllowedIPs = 0.0.0.0/0

# Start WireGuard
sudo wg-quick up wg0

# Enable at boot
sudo systemctl enable wg-quick@wg0
```

---

## 📡 Bluetooth Connectivity

### Bluetooth Not Working

#### Basic Bluetooth Troubleshooting

```bash path=null start=null
# Check Bluetooth status
bluetoothctl show

# Check if Bluetooth is enabled
rfkill list bluetooth

# Enable Bluetooth
sudo rfkill unblock bluetooth

# Start Bluetooth service
sudo systemctl start bluetooth
sudo systemctl enable bluetooth

# Reset Bluetooth
sudo systemctl restart bluetooth
```

#### Bluetooth Device Pairing

```bash path=null start=null
# Enter bluetoothctl
bluetoothctl

# In bluetoothctl prompt:
power on
agent on
default-agent
scan on
# Wait for device to appear
pair XX:XX:XX:XX:XX:XX
trust XX:XX:XX:XX:XX:XX
connect XX:XX:XX:XX:XX:XX
```

### Bluetooth Audio Issues

```bash path=null start=null
# Install PulseAudio Bluetooth module
sudo apt install pulseaudio-module-bluetooth

# Restart PulseAudio
pulseaudio -k
pulseaudio --start

# Check audio devices
pactl list short sinks
pactl list short sources

# Set Bluetooth device as default
pactl set-default-sink bluez_sink.XX_XX_XX_XX_XX_XX.a2dp_sink
```

---

## 🛠️ Advanced Network Tools

### Network Diagnostic Tools

```bash path=null start=null
# Install comprehensive network tools
sudo apt install net-tools dnsutils traceroute nmap tcpdump wireshark

# Network route tracing
traceroute google.com
mtr google.com

# Port scanning
nmap -p 22,80,443 192.168.1.1

# Network packet capture
sudo tcpdump -i any -n host google.com

# Network bandwidth testing between hosts
# On server: iperf3 -s
# On client: iperf3 -c SERVER_IP
```

### Firewall Diagnostics

```bash path=null start=null
# Check UFW status
sudo ufw status verbose

# Check iptables rules
sudo iptables -L -n -v

# Temporarily disable firewall for testing
sudo ufw disable
# Test connection
# Re-enable firewall
sudo ufw enable
```

---

## 🔍 Network Monitoring

### Real-time Network Monitoring

```bash path=null start=null
# Install monitoring tools
sudo apt install nethogs iftop bmon

# Monitor network usage by process
sudo nethogs

# Monitor network bandwidth by interface
sudo iftop

# Monitor network interfaces
bmon

# Watch network statistics
watch -n 1 cat /proc/net/dev
```

### Network Configuration Files

```bash path=null start=null
# Important network configuration files to check:

# Network interfaces (older Ubuntu versions)
cat /etc/network/interfaces

# Netplan configuration (Ubuntu 18.04+)
ls /etc/netplan/

# DNS configuration
cat /etc/resolv.conf
cat /etc/systemd/resolved.conf

# Network hosts
cat /etc/hosts

# NetworkManager connections
ls /etc/NetworkManager/system-connections/
```

---

## 🚨 Emergency Network Recovery

### Network Service Reset

```bash path=null start=null
# Complete network reset
sudo systemctl restart NetworkManager
sudo systemctl restart systemd-resolved

# Or more comprehensive reset
sudo service network-manager restart
sudo service networking restart

# Reset network interfaces
sudo ip link set eth0 down
sudo ip link set eth0 up
```

### Boot from Live USB for Network Recovery

```bash path=null start=null
# If system won't boot or network is completely broken:
# 1. Boot from Ubuntu live USB
# 2. Mount your system drive
sudo mkdir /mnt/system
sudo mount /dev/sda1 /mnt/system

# 3. Chroot into your system
sudo chroot /mnt/system

# 4. Fix network configuration
# Edit network files as needed
# Reinstall network packages if corrupted
apt update && apt install --reinstall network-manager
```

---

## 📋 Network Troubleshooting Checklist

### Step-by-step Diagnosis

1. **Physical Connection**
   - [ ] Check cable connections
   - [ ] Verify LED indicators on network adapter
   - [ ] Test with different cable/port

2. **Interface Status**
   ```bash path=null start=null
   ip link show
   ```
   - [ ] Interface is UP
   - [ ] No error messages

3. **IP Configuration**
   ```bash path=null start=null
   ip addr show
   ```
   - [ ] Valid IP address assigned
   - [ ] Correct subnet mask
   - [ ] IP not conflicting

4. **Gateway Connectivity**
   ```bash path=null start=null
   ip route show
   ping $(ip route | grep default | awk '{print $3}')
   ```
   - [ ] Default gateway configured
   - [ ] Gateway is reachable

5. **DNS Resolution**
   ```bash path=null start=null
   nslookup google.com
   ```
   - [ ] DNS servers configured
   - [ ] DNS resolution working

6. **Internet Connectivity**
   ```bash path=null start=null
   ping -c 4 8.8.8.8
   curl -I http://google.com
   ```
   - [ ] Can reach external IPs
   - [ ] Can access websites

---

## 🎯 Common Network Issues & Solutions

### Issue: "Network Unreachable"
```bash path=null start=null
# Check routing table
ip route show

# Add default route if missing
sudo ip route add default via 192.168.1.1

# Make permanent in NetworkManager
nmcli connection modify "YourConnection" ipv4.gateway 192.168.1.1
```

### Issue: "Temporary failure in name resolution"
```bash path=null start=null
# Fix DNS issues
sudo systemctl restart systemd-resolved

# Add DNS servers
echo 'nameserver 8.8.8.8' | sudo tee -a /etc/resolv.conf
```

### Issue: WiFi Connected but No Internet
```bash path=null start=null
# Reset network settings
nmcli connection down "YourWiFi"
nmcli connection up "YourWiFi"

# Clear DNS cache
sudo systemd-resolve --flush-caches
```

### Issue: Ethernet Not Detected
```bash path=null start=null
# Check if driver is loaded
lspci -v | grep Ethernet

# Reinstall network drivers
sudo apt install --reinstall linux-modules-$(uname -r)
```

---

## 📚 Network Configuration Examples

### Home Router Setup
```bash path=null start=null
# Typical home network configuration
nmcli connection modify "Home" ipv4.method auto
nmcli connection modify "Home" ipv4.dns "192.168.1.1,8.8.8.8"
```

### Office Static IP
```bash path=null start=null
# Static IP for office environment
nmcli connection modify "Office" ipv4.method manual
nmcli connection modify "Office" ipv4.addresses 192.168.10.100/24
nmcli connection modify "Office" ipv4.gateway 192.168.10.1
nmcli connection modify "Office" ipv4.dns "192.168.10.1,8.8.8.8"
```

### Mobile Hotspot
```bash path=null start=null
# Create mobile hotspot
nmcli device wifi hotspot con-name "MyHotspot" ssid "Ubuntu-Hotspot" password "YourPassword"
```

---

> 🌐 **Network Tip**: Always work systematically from the physical layer up to the application layer when troubleshooting network issues. Document your configuration changes for easy rollback.

**Next Steps**: Once your network is stable, secure it with our [Security & Privacy Guide](../advanced/security-privacy.md) or optimize performance with our [Performance Optimization Guide](../advanced/performance-optimization.md).
