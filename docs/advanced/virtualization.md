# 🖥️ QEMU/KVM Virtualization on Ubuntu 24.04

> **Run multiple operating systems simultaneously with near-native performance using QEMU/KVM**

---

## 🧠 What is QEMU/KVM?

* **QEMU** is a generic emulator and virtualizer.
* **KVM (Kernel-based Virtual Machine)** uses hardware acceleration to run VMs almost as fast as native code.
* Together, they provide a **lightweight, fast, and stable** virtualization solution on Linux.

> ✅ Ubuntu 24.04 comes with excellent KVM support.

---

## 🧰 What You'll Get

* Run multiple full Linux VMs (Ubuntu, Fedora, Debian, etc.)
* No need for VirtualBox or its kernel modules
* Works cleanly with Secure Boot, and is well integrated into Ubuntu

---

## ✅ Step-by-Step: Set Up QEMU/KVM on Ubuntu 24.04

### 🔹 Step 1: Check for Virtualization Support

Run:

```bash
egrep -c '(vmx|svm)' /proc/cpuinfo
```

* Output ≥ 1 → your CPU supports virtualization
* If 0 → Enable virtualization in BIOS/UEFI first

---

### 🔹 Step 2: Install QEMU, KVM, and virt-manager

```bash
sudo apt update
sudo apt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager
```

* `virt-manager`: GUI to manage VMs
* `qemu-kvm`: core KVM virtualization
* `libvirt-*`: services that manage VMs

---

### 🔹 Step 3: Add Yourself to `libvirt` and `kvm` Groups

```bash
sudo usermod -aG libvirt $USER
sudo usermod -aG kvm $USER
```

Then **log out and log back in**, or run:

```bash
newgrp libvirt
newgrp kvm
```

---

### 🔹 Step 4: Launch Virt-Manager (GUI)

Start the VM manager:

```bash
virt-manager
```

You'll see a GUI where you can:

* **Create a new VM**
* Choose an ISO file (Ubuntu, Fedora, etc.)
* Allocate RAM, CPU, disk size
* Boot and run your VM

---

### 🔹 Step 5: Download an ISO and Create a VM

1. Download a Linux distro ISO (e.g., [Ubuntu ISO](https://ubuntu.com/download/desktop)).
2. In `virt-manager`:

   * Click **"Create a new virtual machine"**
   * Choose **"Local install media (ISO)"**
   * Browse to the downloaded ISO
   * Assign memory and CPU
   * Create a virtual disk
   * Finish and run the VM

✅ You now have a full, isolated Linux machine.

## ✅ Why Use QEMU/KVM?

| Feature                | Details                                |
| ---------------------- | -------------------------------------- |
| ✅ Fast                 | Near-native performance with KVM       |
| ✅ Stable               | Built into Linux kernel, very reliable |
| ✅ Secure Boot-friendly | No need for signing modules            |
| ✅ GUI & CLI            | Choose your comfort zone               |
| ✅ Flexible             | Supports snapshots, networking, etc.   |

---


## 🛠 Additional Info: Command-Line VM Creation (For Scripted Setup)

You can also create VMs from the terminal using `virt-install`. Here's an example:

```bash
virt-install \
--name ubuntu-vm \
--ram 2048 \
--vcpus=2 \
--os-type=linux \
--os-variant=ubuntu24.04 \
--cdrom=/path/to/ubuntu.iso \
--disk size=20 \
--graphics spice \
--network network=default \
--virt-type=kvm
```

---

## 🔄 VM Management Tips

| Task             | How to do it                                    |
| ---------------- | ----------------------------------------------- |
| Start/stop VM    | From `virt-manager` or with `virsh` CLI         |
| List VMs         | `virsh list --all`                              |
| Delete a VM      | `virsh undefine <vm-name> --remove-all-storage` |
| VM storage files | Located in `/var/lib/libvirt/images/`           |

---

## 🚨 Troubleshooting

### Permission Issues
If you get permission errors:
```bash
# Ensure your user is in the correct groups
groups $USER

# Restart libvirt service
sudo systemctl restart libvirtd
```

### Performance Optimization
For better VM performance:
- Enable **nested virtualization** if running VMs inside VMs
- Allocate sufficient RAM (at least 2GB for modern Linux distros)
- Use **VirtIO** drivers for better disk and network performance

### Networking Issues
If VMs can't access the internet:
```bash
# Check if default network is active
virsh net-list --all

# Start default network if inactive
virsh net-start default
virsh net-autostart default
```

---

## 📚 Next Steps

- **Advanced Networking**: Set up bridged networking for production-like setups
- **Snapshots**: Use `virsh snapshot-create` to save VM states
- **Automation**: Script VM deployments using `virt-install` and cloud-init
- **GPU Passthrough**: For advanced users wanting GPU acceleration in VMs

---

> 💡 **Pro Tip**: QEMU/KVM is the foundation for many cloud platforms (OpenStack, Proxmox). Learning it gives you valuable skills for cloud infrastructure!
