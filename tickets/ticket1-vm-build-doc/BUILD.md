# BUILD GUIDE — Linux and Windows Virtual Machines

**Hypervisor:** Oracle VirtualBox  
**Host:** Windows 10/11  
**Host RAM:** 16 GB  
**Host CPU:** 6 cores

## Purpose

This document explains how to create two virtual machines from scratch:

1. Ubuntu Server 26.04.1 LTS.
2. Windows 11 Enterprise Evaluation.

Both virtual machines will use a shared NAT Network to access the internet and communicate with each other.

The instructions are written for a person who has never used a hypervisor before.

---

## Step 1. Prepare the required software

Before creating virtual machines, install Oracle VirtualBox on the Windows host.

Download VirtualBox from:

https://www.virtualbox.org/wiki/Downloads

Download the operating system installation files:

- Ubuntu Server: https://ubuntu.com/download/server
- Windows 11 Enterprise Evaluation: https://www.microsoft.com/en-us/evalcenter/download-windows-11-enterprise

Save both ISO files in an easily accessible folder.

**Why:** ISO files are installation media for virtual computers. Downloading them before creating the VMs avoids interrupting the installation process.

**Screenshot:** `evidence/01-new-vm-wizard.png`

Capture the VirtualBox interface with the New VM wizard open.

---

## Step 2. Create the Ubuntu virtual machine

Open Oracle VirtualBox.

1. Click **New**.
2. Enter the name `ticket1-ubuntu`.
3. Select the Ubuntu Server ISO file.
4. Select Linux as the operating system type.
5. Select Ubuntu (64-bit).
6. Configure the following resources:

| Setting | Value |
|---|---|
| CPU | 1 vCPU |
| RAM | 2048 MB |
| Disk | 25 GB |
| Disk type | VDI |
| Allocation | Dynamically allocated |

7. Complete the wizard.

**Why:** Ubuntu Server does not require a graphical desktop for this assignment. One CPU core and 2 GB RAM are sufficient for basic command-line administration and network testing. A dynamically allocated disk avoids reserving all 25 GB immediately.

**Screenshot:** Capture the completed VM configuration.

Save as `evidence/01-new-vm-wizard.png`.

---

## Step 3. Configure the Ubuntu virtual disk

Open the Ubuntu VM settings.

1. Select the VM.
2. Open **Settings**.
3. Select **Storage**.
4. Check that the virtual disk is 25 GB.
5. Confirm that the disk uses VDI format and dynamic allocation.

**Why:** A separate virtual disk stores the guest operating system independently of the host. Dynamic allocation saves physical disk space while the guest is not using the full virtual capacity.

**Screenshot:** Save the disk configuration as `evidence/02-disk-settings.png`.

---

## Step 4. Create the Windows virtual machine

In VirtualBox, click **New**.

Enter the following configuration:

| Setting | Value |
|---|---|
| VM name | ticket1-windows |
| Operating system | Windows 11 |
| CPU | 2 vCPU |
| RAM | 4096 MB |
| Disk | 64 GB |
| Disk format | VDI |
| Allocation | Dynamically allocated |
| Firmware | UEFI |
| Secure Boot | Enabled |
| TPM | Version 2.0 |

Select the Windows 11 Enterprise Evaluation ISO file.

Complete the wizard.

**Why:** Windows requires more resources than Ubuntu Server. I allocated 4 GB RAM and 2 CPU cores because Windows has a graphical interface and requires more resources for installation and normal operation.

UEFI, Secure Boot and TPM 2.0 are configured to meet Windows 11 requirements.

**Screenshot:** Capture the Windows VM creation settings.

---

## Step 5. Configure the shared NAT Network

Both virtual machines must use the same network.

First, create the network:

1. Open VirtualBox.
2. Open **Tools** or **File → Tools**, depending on the VirtualBox version.
3. Open **Network Manager**.
4. Select **NAT Networks**.
5. Click **Create**.
6. Name the network `Ticket1-NAT`.
7. Enable DHCP.

Next, configure the Ubuntu VM:

1. Select `ticket1-ubuntu`.
2. Open **Settings → Network**.
3. Enable Adapter 1.
4. Select **NAT Network**.
5. Choose `Ticket1-NAT`.

Repeat these steps for `ticket1-windows`.

**Why NAT Network and not Standard NAT:** Standard NAT provides internet access, but separate NAT adapters do not normally allow direct guest-to-guest communication.

**Why NAT Network and not Bridged:** Bridged networking exposes the guests directly to the physical network and can be restricted by university Wi-Fi.

NAT Network provides internet access while allowing both guests to communicate through a shared virtual subnet.

**Screenshot:** Save the network configuration as `evidence/03-network-adapter.png`.

---

## Step 6. Install Ubuntu Server

Start `ticket1-ubuntu`.

1. Select the Ubuntu Server installer.
2. Choose the language.
3. Select the keyboard layout.
4. Configure the network using DHCP.
5. Select the default storage layout on the virtual disk.
6. Create a username and password.
7. Complete the installation.
8. Restart the VM.
9. Remove the installation ISO if VirtualBox requests it.

Log in using the account created during installation.

Run:

```bash
uname -a
```

Then run:

```bash
cat /etc/os-release
```

**Why:** These commands confirm the running Linux kernel and installed operating system. Verifying the installation is more reliable than assuming that a completed installer means everything works correctly.

Save the command output as `evidence/04-guest-uname.txt`.

---

## Step 7. Install Windows 11

Start `ticket1-windows`.

1. Select the language.
2. Choose the keyboard layout.
3. Start Windows installation.
4. Select the Windows 11 Enterprise Evaluation edition if prompted.
5. Accept the licence terms.
6. Select the virtual disk as the installation destination.
7. Complete the installation.
8. Create a local user account if the installer allows it.
9. Complete the initial Windows setup.
10. Log in to the Windows desktop.

Open PowerShell.

Run:

```powershell
winver
```

Then run:

```powershell
ipconfig
```

**Why:** These commands verify the installed Windows version and network configuration.

---

## Step 8. Verify internet connectivity

Start both virtual machines.

In Ubuntu, run:

```bash
ping -c 4 8.8.8.8
```

In Windows PowerShell, run:

```powershell
ping 8.8.8.8
```

Both guests should receive replies.

If the tests fail, check that:

- Both adapters are enabled.
- Both guests use the same NAT Network.
- DHCP is enabled.
- The host has an active internet connection.

**Why:** A working virtual network adapter does not automatically prove that the guest can access the internet. Actual connectivity tests are necessary.

---

## Step 9. Test communication between guests

First, find the Ubuntu IP address:

```bash
ip -br addr
```

Then find the Windows IP address:

```powershell
ipconfig
```

Write down the actual IPv4 addresses.

From Ubuntu, test the Windows address:

```bash
ping -c 4 WINDOWS_IP_ADDRESS
```

From Windows, test the Ubuntu address:

```powershell
ping -n 4 UBUNTU_IP_ADDRESS
```

Replace the placeholders with the actual addresses.

If Windows does not respond, check Windows Defender Firewall. Windows may block incoming ICMP requests on a Public network profile.

If necessary, create an inbound ICMPv4 rule using an administrator PowerShell session:

```powershell
New-NetFirewallRule -DisplayName "Allow ping in (ICMPv4)" -Protocol ICMPv4 -IcmpType 8 -Direction Inbound -Action Allow
```

Repeat the ping tests.

**Why:** Successful internet access does not prove that two virtual machines can communicate with each other. Both tests are required.

Save the actual command output in `evidence/05-ping-between-guests.txt`.

---

## Step 10. Create clean snapshots

After both operating systems are installed and their basic configuration is complete:

1. Shut down the Ubuntu guest.
2. Open its Snapshots view.
3. Create a snapshot named `ubuntu-clean-install`.
4. Start Windows and shut it down.
5. Create a snapshot named `windows-clean-install`.

**Why:** Snapshots provide a recovery point before potentially destructive testing. They should be created before modifying system files or installing experimental software.

**Screenshot:** Save the snapshot list as `evidence/06-snapshot-list.png`.

---

## Step 11. Final verification

Before considering the environment complete, verify the following:

- [ ] Ubuntu starts successfully.
- [ ] Windows starts successfully.
- [ ] Ubuntu has internet access.
- [ ] Windows has internet access.
- [ ] Ubuntu can communicate with Windows.
- [ ] Windows can communicate with Ubuntu.
- [ ] Both virtual machines have clean snapshots.
- [ ] Both virtual disks have the expected configuration.
- [ ] The host remains responsive while both guests are running.

The build is complete only after all checks have been performed successfully.

---

## Troubleshooting

### Problem 1. Ubuntu keeps its old IP address

**Cause:** The guest may retain an old DHCP lease after changing its network adapter.

**Solution:** Shut down the VM completely and restart it. Check the IP address again using:

```bash
ip addr
```

### Problem 2. Windows does not respond to ping

**Cause:** Windows Defender Firewall may block incoming ICMP requests.

**Solution:** Check the firewall profile and create an appropriate inbound ICMPv4 rule if required.

### Problem 3. Host becomes very slow

**Cause:** Too much memory has been allocated to the guests.

**Solution:** Shut down the VMs and reduce their memory allocation. Avoid assigning more memory than the host can comfortably provide.

---

**End of build guide.**
