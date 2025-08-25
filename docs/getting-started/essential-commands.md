# ⚡ Essential Linux Commands

> **Master the command line with this comprehensive guide to must-know Ubuntu commands**

## 📋 Table of Contents

- [🚀 Getting Started](#-getting-started)
- [📁 File & Directory Management](#-file--directory-management)
- [🔧 System Control](#-system-control)
- [📦 Package Management](#-package-management)
- [🔍 Information & Monitoring](#-information--monitoring)
- [💡 Pro Tips](#-pro-tips)

---

## 🚀 Getting Started

### Auto-Completion Magic
**Tab Completion** is your best friend! Press `Tab` to auto-complete commands, filenames, and paths.

```bash path=null start=null
# Type 'reb' and press Tab
reb<Tab>  # Completes to 'reboot'

# Tab completion also works for filenames
ls Doc<Tab>  # Completes to 'Documents/'
```

### Command History Navigation
- **↑/↓ arrows**: Navigate through previous commands
- **Ctrl+R**: Search command history
- **!!**: Repeat last command
- **!n**: Execute command number n from history

---

## 📁 File & Directory Management

### 📂 Directory Navigation

| Command | Description | Example |
|---------|-------------|---------|
| `pwd` | Show current directory | `pwd` |
| `ls` | List files and directories | `ls -la` |
| `cd` | Change directory | `cd Documents/` |
| `cd ..` | Go back one directory | `cd ..` |
| `cd ~` | Go to home directory | `cd ~` |
| `cd -` | Go to previous directory | `cd -` |

### 📝 Creating Files & Directories

```bash path=null start=null
# Create a new directory
mkdir MyProject
mkdir -p path/to/nested/directory  # Create nested directories

# Create a new file
touch filename.txt
touch file1.txt file2.txt file3.txt  # Create multiple files

# Create file with content
echo "Hello World" > greeting.txt
```

### 📋 Listing Files

```bash path=null start=null
# Basic listing
ls                    # List files and directories
ls -l                 # Long format (detailed)
ls -la                # Include hidden files
ls -lh                # Human-readable file sizes
ls -lt                # Sort by modification time
ls -lS                # Sort by file size

# List specific types
ls *.txt              # List all .txt files
ls -d */              # List only directories
```

### 🗑️ Removing Files & Directories

> ⚠️ **Warning**: Be careful with `rm` commands - deleted files cannot be easily recovered!

```bash path=null start=null
# Remove files
rm filename.txt                    # Remove single file
rm file1.txt file2.txt            # Remove multiple files
rm *.txt                          # Remove all .txt files

# Remove directories
rmdir empty_directory              # Remove empty directory only
rm -r directory_name               # Remove directory and contents
rm -rf directory_name              # Force remove (use with caution!)
```

### 📄 File Operations

```bash path=null start=null
# Copy files
cp source.txt destination.txt     # Copy file
cp -r source_dir/ dest_dir/       # Copy directory recursively

# Move/rename files
mv oldname.txt newname.txt        # Rename file
mv file.txt /path/to/destination/  # Move file

# View file contents
cat file.txt                      # Display entire file
less file.txt                     # View file page by page (q to quit)
head file.txt                     # Show first 10 lines
tail file.txt                     # Show last 10 lines
tail -f logfile.txt               # Follow file changes (useful for logs)
```

---

## 🔧 System Control

### 🔄 System Power Management

```bash path=null start=null
# Restart system
sudo reboot

# Shutdown system
sudo poweroff
sudo shutdown -h now              # Immediate shutdown
sudo shutdown -h +10              # Shutdown in 10 minutes
sudo shutdown -r +5               # Restart in 5 minutes

# Cancel scheduled shutdown
sudo shutdown -c
```

### 👤 User & Permissions

```bash path=null start=null
# User information
whoami                            # Show current username
id                                # Show user and group IDs
groups                            # Show groups you belong to

# File permissions
chmod +x script.sh                # Make file executable
chmod 755 file.txt                # Set specific permissions
chmod -R 644 directory/           # Apply permissions recursively

# Change ownership
sudo chown user:group file.txt    # Change owner and group
sudo chown -R user:group dir/     # Apply recursively
```

### 🔍 Process Management

```bash path=null start=null
# View processes
ps aux                            # List all running processes
pgrep firefox                     # Find process ID by name
pidof firefox                     # Alternative to find PID

# Manage processes
kill PID                          # Terminate process by ID
killall firefox                   # Terminate all firefox processes
sudo kill -9 PID                  # Force kill process

# System monitoring
top                               # Real-time process viewer
htop                              # Enhanced process viewer (if installed)
```

---

## 📦 Package Management

### 📥 Installing & Updating Software

```bash path=null start=null
# Update package lists
sudo apt update

# Upgrade installed packages
sudo apt upgrade                  # Upgrade all packages
sudo apt upgrade -y               # Upgrade without prompting

# Combined update and upgrade
sudo apt update && sudo apt upgrade -y

# Install software
sudo apt install package_name
sudo apt install package1 package2 package3  # Install multiple packages
```

### 📦 Package Operations

```bash path=null start=null
# Install .deb packages
sudo apt install ./package_name.deb
sudo dpkg -i package_name.deb    # Alternative method

# Search for packages
apt search keyword
apt list --installed | grep package_name

# Remove packages
sudo apt remove package_name      # Remove package (keep config files)
sudo apt purge package_name       # Remove package and config files
sudo apt autoremove              # Remove unused dependencies

# Package information
apt show package_name             # Show package details
dpkg -l | grep package           # List installed packages matching pattern
```

---

## 🔍 Information & Monitoring

### 💾 System Information

```bash path=null start=null
# System details
uname -a                          # System information
lsb_release -a                    # Ubuntu version details
hostnamectl                       # System hostname and OS info

# Hardware information
lscpu                             # CPU information
free -h                           # Memory usage (human-readable)
df -h                             # Disk space usage
lsblk                             # List block devices
lsusb                             # List USB devices
lspci                             # List PCI devices
```

### 🌐 Network Information

```bash path=null start=null
# Network interfaces
ip addr show                      # Show IP addresses
ifconfig                          # Network interface configuration (if installed)

# Network connectivity
ping google.com                   # Test internet connection
wget URL                          # Download files from web
curl URL                          # Transfer data from/to servers
```

### 📊 File & Directory Information

```bash path=null start=null
# File information
file filename.txt                 # Determine file type
stat filename.txt                 # Detailed file information
ls -lh                            # File sizes in human-readable format

# Directory sizes
du -sh directory/                 # Show directory size
du -h --max-depth=1              # Show sizes of subdirectories
```

---

## 💡 Pro Tips

### ⚡ Command Shortcuts

```bash path=null start=null
# Navigation shortcuts
cd ~                              # Go to home directory
cd -                              # Go to previous directory
pushd /path && popd               # Save and restore directory

# Command history
!!                                # Repeat last command
!n                                # Execute command number n from history
!string                           # Execute last command starting with "string"

# Command editing
Ctrl+A                            # Move cursor to beginning of line
Ctrl+E                            # Move cursor to end of line
Ctrl+K                            # Delete from cursor to end of line
Ctrl+U                            # Delete entire line
```

### 🔗 Command Chaining

```bash path=null start=null
# Sequential execution
command1 && command2              # Run command2 only if command1 succeeds
command1 || command2              # Run command2 only if command1 fails
command1; command2                # Run both commands regardless

# Input/Output redirection
command > file.txt                # Redirect output to file (overwrite)
command >> file.txt               # Redirect output to file (append)
command < input.txt               # Use file as input
command1 | command2               # Pipe output of command1 to command2
```

### 🎯 Useful Combinations

```bash path=null start=null
# System maintenance
sudo apt update && sudo apt upgrade -y && sudo apt autoremove

# Find and process files
find . -name "*.txt" -exec rm {} \;    # Find and delete all .txt files
find . -type f -size +100M              # Find files larger than 100MB

# Archive and compress
tar -czf archive.tar.gz directory/      # Create compressed archive
tar -xzf archive.tar.gz                 # Extract compressed archive
```

---

## 🆘 Getting Help

Every command has built-in help:

```bash path=null start=null
man command_name                  # Detailed manual page
command_name --help               # Quick help summary
info command_name                 # Info documentation
which command_name                # Show command location
```

### 📚 Essential Manual Pages to Explore

- `man ls` - File listing options
- `man chmod` - File permissions
- `man find` - File searching
- `man grep` - Text pattern matching
- `man tar` - Archive management

---

> 💡 **Remember**: Practice these commands regularly, and don't be afraid to experiment in a safe environment. The terminal becomes incredibly powerful once you master these basics!

**Next Steps**: Ready for more? Check out our [Advanced Commands Guide](../advanced/advanced-commands.md) or explore [Productivity Tips](../productivity/).
