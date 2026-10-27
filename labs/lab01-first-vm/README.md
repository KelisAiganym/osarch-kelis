# Lab 1 — Your first virtual machine

**Pair: Kelis Aiganym / Aruzhan**

**Driver first half: Aiganym**

**Machine: Windows host, VirtualBox, Ubuntu Server 26.04.1 LTS guest**

**Date: 25.10.2026**

**WHAT I DID**

Installed VirtualBox.
Created VM lab01-ubuntu with 1 vCPU, 2 GB RAM, a 25 GB dynamically allocated VDI disk, and NAT networking.
Installed Ubuntu Server 26.04.1 LTS and created a snapshot named clean-install.
Broke the VM intentionally by deleting the /etc/hostname file:

cat /etc/hostname — checked the hostname before making changes.

sudo rm /etc/hostname — deleted the hostname file.

cat /etc/hostname — confirmed that the file was missing.

sudo reboot — restarted the virtual machine.

hostname — checked the hostname after rebooting.

Restored the VM from the clean-install snapshot using VirtualBox's Snapshots menu. After the rollback, I logged in and ran cat /etc/hostname and hostname to verify that the original hostname had been restored.

**RESOURCE JUSTIFICATION**

I allocated 1 vCPU and 2 GB of RAM to the Ubuntu Server virtual machine. Although these are the default settings in VirtualBox, they are suitable for this lab because Ubuntu Server requires relatively few resources for basic installation, command-line operations, SSH access, and snapshot testing.

My Windows host has 6 CPU cores and 16 GB of RAM. Allocating 1 vCPU and 2 GB of RAM provides sufficient resources for the guest operating system while leaving most of the CPU and memory available for Windows and other applications. This helps the host maintain good performance while running the virtual machine. Since the lab does not require a graphical desktop or resource-intensive applications, allocating more CPU cores or RAM is unnecessary.

The virtual disk has a maximum size of 25 GB and is dynamically allocated, meaning it uses physical disk space as needed rather than reserving the entire 25 GB immediately. The VM uses NAT networking to access the network through the host without requiring a separate network configuration.

**RESULT**

The Ubuntu Server virtual machine boots successfully. I created the clean-install snapshot before making changes, intentionally broke the VM, and restored it from the snapshot.

**WHAT DID NOT WORK**

The first time I tried to connect to the virtual machine using SSH, the connection was refused because the OpenSSH server was not installed or running in the guest system. I fixed this issue by running `sudo apt update` and `sudo apt install -y openssh-server`, then tested the SSH connection again. After that, I was able to retrieve the Ubuntu version information from the guest.

Another issue occurred during the snapshot rollback. The clean-install snapshot was created at 18:32, one minute after `/etc/hostname` was deleted at 18:31. Therefore, this snapshot could not restore the VM to its original clean-install state. This showed that the snapshot needed to be created before making any changes to the system.

**EVIDENCE**

[evidence/vm-settings.png](evidence/vm-settings.png) - VM CPU, memory, and disk settings.

[evidence/guest-release.png](evidence/guest-release.png) - Ubuntu Server version information.

[evidence/broken.png](evidence/broken.png) - the VM in its broken state.

[evidence/snapshot-list.png](evidence/snapshot-list.png) - the clean-install snapshot after rollback.

[evidence/restored-after-rollback.png](evidence/restored-after-rollback.png) - additional screenshot of the restored VM.
