Lab 1 — Your first virtual machine

Pair: Kelis Aiganym / Aruzhan
Driver first half: Aiganym
Machine: Windows host, VirtualBox, Ubuntu Server 26.04.1 LTS guest
Date: 25.10.2026

What I did
Installed VirtualBox
Created VM lab01-ubuntu with 1 vCPU, 2 GB RAM, a 25 GB dynamically allocated VDI disk, and NAT networking.
Installed Ubuntu Server 26.04.1 LTS and created a snapshot named clean-install.
Broke the VM intentionally: Broke the VM intentionally by deleting the /etc/hostname file:
    cat /etc/hostname — checked the hostname before making changes.
    sudo rm /etc/hostname — deleted the hostname file.
    cat /etc/hostname — confirmed that the file was missing.
    sudo reboot — restarted the virtual machine.
    hostname — checked the hostname after rebooting.
Restored the VM from the clean-install snapshot using VirtualBox's Snapshots menu. After the rollback, I logged in and ran cat /etc/hostname and hostname to verify that the original hostname had been restored.
Resource justification

I allocated 1 vCPU and 2 GB of RAM to the Ubuntu Server virtual machine. My Windows host has 6 CPU cores and 15 GB of RAM. I chose these settings to limit resource usage on my host while leaving enough resources for Windows and other applications to run smoothly. The virtual disk has a maximum size of 25 GB and is dynamically allocated, so it uses physical disk space as needed. The VM uses NAT networking to access the network through the host.

Result

The Ubuntu Server virtual machine boots successfully. I created the clean-install snapshot before making changes, intentionally broke the VM, and restored it from the snapshot.

What did not work the first time

The first time I tried to connect to the virtual machine using SSH, the connection was refused. I found that the OpenSSH server was not installed or running in the guest system. I fixed the issue by running sudo apt update and sudo apt install -y openssh-server, then checked the SSH connection again. After that, I was able to retrieve the Ubuntu version information from the guest.

Evidence
evidence/vm-settings.png — VM CPU, memory, and disk settings.
evidence/guest-release.png — Ubuntu Server version information.
evidence/broken.png — the VM in its broken state.
evidence/snapshot-list.png — the clean-install snapshot after rollback.
evidence/restored-after-rollback.png — additional screenshot of the restored VM.
