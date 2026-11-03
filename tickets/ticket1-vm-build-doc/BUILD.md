VM Build Guide — Ubuntu Server and Windows 11
1. Introduction

This document explains how to create two virtual machines using Oracle VirtualBox on a Windows host computer.

The environment contains:

Ubuntu Server 26.04.1 LTS
Windows 11 Enterprise Evaluation

The purpose of this environment is to provide a safe and isolated place for software development and testing. Both virtual machines will have internet access and will be connected through the same NAT Network.

This guide is written for a person who has never created a virtual machine before. Follow the steps in order and do not skip the verification stages.

2. Requirements
2.1 Host computer

The host computer used for this configuration has:

Component	Specification
Operating system	Windows
RAM	16 GB
CPU	6 cores
Hypervisor	Oracle VirtualBox
Available disk space	Must be checked before installation

Before starting, make sure that the computer has enough free disk space for both virtual machines.

The two virtual disks have a combined maximum capacity of 89 GB. Because they are dynamically allocated, they do not immediately occupy the entire amount. However, sufficient free storage must be available as the virtual machines grow.

2.2 Required downloads

Download the following software from official websites:

Oracle VirtualBox

https://www.virtualbox.org/wiki/Downloads

Ubuntu Server 26.04.1 LTS

https://ubuntu.com/download/server

Windows 11 Enterprise Evaluation

https://www.microsoft.com/en-us/evalcenter/download-windows-11-enterprise

Use the official installation images rather than downloading operating systems from unofficial websites.

3. Virtual machine configuration

The following settings will be used throughout the build.

3.1 Ubuntu Server
Setting	Value
VM name	lab01-ubuntu
Operating system	Ubuntu Server 26.04.1 LTS
CPU	1 vCPU
RAM	2 GB
Virtual disk	25 GB
Disk type	Dynamically allocated VDI
Network	NAT Network

Ubuntu Server does not require a graphical desktop for this testing environment. Therefore, 1 CPU core and 2 GB of RAM are sufficient for basic command-line operations.

3.2 Windows 11
Setting	Value
VM name	lab02-windows
Operating system	Windows 11 Enterprise Evaluation
CPU	2 vCPU
RAM	4 GB
Virtual disk	64 GB
Disk type	Dynamically allocated VDI
Firmware	UEFI
Secure Boot	Enabled
TPM	Version 2.0
Network	NAT Network

Windows receives more resources because it has higher system requirements than Ubuntu Server.

The 4 GB RAM allocation meets the minimum Windows 11 memory requirement, although heavier applications may require more.

4. Installing Oracle VirtualBox
Step 1. Download VirtualBox

Open the official VirtualBox website and download the Windows host installer.

Run the downloaded installer and follow the installation wizard.

Keep the standard installation components unless there is a specific reason to change them.

Why: Using the standard installation components avoids unnecessary configuration changes and provides the networking and virtualization features required for this environment.

Screenshot: evidence/01-virtualbox-installation.png

Step 2. Open VirtualBox

After installation, open Oracle VirtualBox from the Windows Start menu.

The main window displays the list of virtual machines. Initially, this list may be empty.

Why: VirtualBox acts as the hypervisor. It manages the guest operating systems and provides their virtual hardware.

Screenshot: evidence/02-virtualbox-main-window.png

5. Creating the shared NAT Network
Step 3. Open Network Manager

In VirtualBox, open:

File → Tools → Network Manager

Select the NAT Networks section.

Click Create to add a new NAT Network.

Name the network:

DevTestNetwork

Use a private subnet, for example:

10.0.2.0/24

Enable DHCP.

If your VirtualBox version displays different controls, use the equivalent NAT Network creation settings.

Why: A shared NAT Network allows both virtual machines to communicate with each other while accessing external networks through the host. Standard NAT would not provide the same direct guest-to-guest connectivity.

Screenshot: evidence/03-nat-network-creation.png

Step 4. Verify the NAT Network

Check that:

The network name is DevTestNetwork.
The network is enabled.
DHCP is enabled.
The network uses the selected private subnet.

Save the configuration.

Why: Both guests must use the same network profile. If they use separate NAT Network profiles, they may have internet access but will not automatically communicate with each other.

Screenshot: evidence/04-nat-network-settings.png

6. Creating the Ubuntu Server virtual machine
Step 5. Start the VM creation wizard

In VirtualBox, click New.

Enter:

Name: lab01-ubuntu
ISO Image: Select the downloaded Ubuntu Server installation image.
Type: Linux
Version: Ubuntu (64-bit), or the closest available Ubuntu 64-bit option.

If VirtualBox offers unattended installation, use manual installation if you want to follow the Ubuntu installer yourself.

Why: Manual installation makes the process easier to understand and document. It also allows the administrator to control the user account and disk configuration.

Screenshot: evidence/05-ubuntu-vm-wizard.png

Step 6. Allocate memory and CPU

Set the following values:

Base Memory: 2048 MB
Processors: 1

Do not allocate more resources than necessary.

Why: Ubuntu Server is lightweight and does not need many resources for basic development and network testing. Keeping the allocation low helps the host remain responsive.

Screenshot: evidence/06-ubuntu-resource-settings.png

Step 7. Create the virtual hard disk

Choose:

Create a virtual hard disk now.
Disk file type: VDI.
Storage on physical hard disk: Dynamically allocated.
Maximum disk size: 25 GB.

Complete the wizard.

Why: A dynamically allocated disk grows as the guest stores data. A fixed-size disk would reserve all 25 GB immediately, even if the guest uses only a small amount.

Screenshot: evidence/07-ubuntu-disk-settings.png

Step 8. Configure the Ubuntu network adapter

Select the Ubuntu VM and open:

Settings → Network → Adapter 1

Enable the network adapter.

Set Attached to:

NAT Network

Select:

DevTestNetwork

Save the settings.

Why: The Ubuntu guest must use the same NAT Network as Windows to communicate with the second guest.

Screenshot: evidence/08-ubuntu-network-settings.png

Step 9. Install Ubuntu Server

Start the virtual machine.

Follow the Ubuntu installation wizard.

Select the installation language.
Select the keyboard layout.
Configure the network using DHCP.
Select the default storage layout using the virtual disk.
Create a username and password.
Select the OpenSSH server installation option if available.
Complete the installation.
Restart the virtual machine when prompted.

Remove the installation ISO when VirtualBox requests it or when the installer finishes.

Why: DHCP automatically obtains network settings from the virtual network. OpenSSH provides remote access to the Linux guest without requiring direct access to its console.

Screenshot: evidence/09-ubuntu-installation.png

Step 10. Update Ubuntu and verify the installation

Log in to Ubuntu and run:

bash
sudo apt update
sudo apt upgrade -y


Check the operating system:

bash
cat /etc/os-release


Check the kernel:

bash
uname -a


Check the network interfaces:

bash
ip addr


Check the routing table:

bash
ip route


Install the SSH server if it was not installed during setup:

bash
sudo apt install -y openssh-server


Check its status:

bash
sudo systemctl status ssh


Why: Updating the system reduces the risk of using outdated packages. These commands verify that the operating system is installed and that the network interface has received its configuration.

Screenshot: evidence/10-ubuntu-verification.png

7. Creating the Windows 11 virtual machine
Step 11. Start the Windows VM creation wizard

In VirtualBox, click New.

Enter:

Name: lab02-windows
ISO Image: Select the Windows 11 Enterprise Evaluation ISO.
Type: Microsoft Windows.
Version: Windows 11 (64-bit), or the closest available option.

Select manual installation if the wizard offers unattended installation and you want to configure Windows yourself.

Screenshot: evidence/11-windows-vm-wizard.png

Step 12. Configure Windows resources

Set:

Base Memory: 4096 MB
Processors: 2

Why: Windows 11 requires more memory than the Ubuntu Server guest. Allocating 4 GB meets its minimum RAM requirement while leaving most host memory available.

Screenshot: evidence/12-windows-resource-settings.png

Step 13. Configure Windows firmware and security

Open the VM settings.

Under System → Motherboard:

Enable EFI (UEFI).
Enable Secure Boot if supported by the installed VirtualBox version.

Under Security or the corresponding virtual machine settings:

Enable TPM.
Select TPM 2.0.

The exact menu location may vary between VirtualBox versions.

Why: Windows 11 requires UEFI-compatible firmware, Secure Boot capability and TPM 2.0 support. These settings allow the guest to meet the operating system's security requirements.

Screenshot: evidence/13-windows-security-settings.png

Step 14. Create the Windows virtual disk

Create a new virtual hard disk with these settings:

File type: VDI.
Allocation: Dynamically allocated.
Maximum size: 64 GB.

Why: Windows requires more storage than the basic Ubuntu Server installation because of system updates, applications and development tools.

Dynamic allocation avoids reserving the entire disk capacity immediately.

Screenshot: evidence/14-windows-disk-settings.png

Step 15. Configure the Windows network

Open:

Settings → Network → Adapter 1

Enable the adapter.

Set Attached to:

NAT Network

Select:

DevTestNetwork

Save the configuration.

Why: Both guests must use the same NAT Network to communicate with each other and access the internet.

Screenshot: evidence/15-windows-network-settings.png

Step 16. Install Windows 11

Start the Windows VM.

Follow the Windows installation wizard:

Select the language and regional settings.
Select the keyboard layout.
Start the installation.
Choose the Windows 11 Enterprise Evaluation edition if the installer requests an edition.
Select the virtual disk as the installation destination.
Complete the installation.
Configure the initial Windows account.
Finish the initial setup.

Use the evaluation installation according to Microsoft's licensing and evaluation terms.

Why: The Windows guest provides a separate environment for testing applications that require the Windows operating system.

Screenshot: evidence/16-windows-installation.png

Step 17. Verify Windows installation

Open PowerShell inside the Windows guest.

Run:

powershell
systeminfo


Check the IP configuration:

powershell
ipconfig /all


Check the routing table:

powershell
route print


Check DNS:

powershell
nslookup google.com


Why: These commands provide information about the installed operating system, network adapter, IP address and DNS configuration.

Screenshot: evidence/17-windows-verification.png

8. Testing network connectivity
Step 18. Check internet access from Ubuntu

Inside Ubuntu, run:

bash
ping -c 4 1.1.1.1


Then test DNS resolution:

bash
ping -c 4 google.com


If both commands succeed, the guest can reach an external IP address and resolve domain names.

Also test HTTPS connectivity:

bash
curl -I https://ubuntu.com


Why: Testing both an IP address and a domain name helps distinguish basic network connectivity problems from DNS problems.

Screenshot: evidence/18-ubuntu-internet-test.png

Step 19. Check internet access from Windows

Open PowerShell and run:

powershell
ping 1.1.1.1


Then:

powershell
nslookup google.com


Test HTTPS connectivity:

powershell
Test-NetConnection ubuntu.com -Port 443


The TCP test should show:

TcpTestSucceeded : True

Why: These tests verify that Windows can access external networks and resolve domain names.

Screenshot: evidence/19-windows-internet-test.png

Step 20. Find the IP addresses of both guests

Inside Ubuntu, run:

bash
ip -br addr


Record the IPv4 address of the active network interface.

Inside Windows PowerShell, run:

powershell
ipconfig


Record the IPv4 address of the Ethernet adapter.

Both addresses should belong to the same NAT Network subnet.

Why: Recording the IP addresses makes it possible to test communication between the two guests.

Screenshot: evidence/20-guest-ip-addresses.png

Step 21. Test communication from Ubuntu to Windows

Inside Ubuntu, run:

bash
ping -c 4 WINDOWS_IP


Replace WINDOWS_IP with the actual Windows guest IP address.

For example:

bash
ping -c 4 10.0.2.4


The expected result is four successful replies.

Why: This verifies that the Linux guest can communicate with the Windows guest over the shared NAT Network.

Screenshot: evidence/21-ubuntu-to-windows-ping.png

Step 22. Test communication from Windows to Ubuntu

Inside Windows PowerShell, run:

powershell
ping -n 4 UBUNTU_IP


Replace UBUNTU_IP with the actual Ubuntu guest IP address.

For example:

powershell
ping -n 4 10.0.2.5


The expected result is successful replies from Ubuntu.

If Windows blocks incoming ping requests, check Windows Defender Firewall.

Open PowerShell as Administrator and create a specific inbound rule:

powershell
New-NetFirewallRule `
  -DisplayName "Allow ping in (ICMPv4)" `
  -Protocol ICMPv4 `
  -IcmpType 8 `
  -Direction Inbound `
  -Action Allow


Do not disable the entire firewall.

If Ubuntu has an active firewall that blocks incoming ICMP requests, investigate its rules before changing them.

Why: Windows Defender Firewall may block incoming ICMP requests even when the network itself is working. A specific rule allows the required connectivity test without disabling other firewall protections.

Screenshot: evidence/22-windows-to-ubuntu-ping.png

9. Creating clean snapshots
Step 23. Prepare Ubuntu for its clean snapshot

Make sure Ubuntu:

Boots successfully.
Has completed its initial updates.
Has the correct hostname.
Has working network connectivity.
Has the required packages installed.

Shut down Ubuntu properly:

bash
sudo poweroff


In VirtualBox, select the VM and open the Snapshots view.

Click Take Snapshot.

Name it:

clean-install

Add a description:

Clean Ubuntu Server installation with initial configuration and network verification.

Why: A snapshot taken before destructive testing provides a known working recovery point.

Screenshot: evidence/23-ubuntu-clean-snapshot.png

Step 24. Prepare Windows for its clean snapshot

Make sure Windows:

Boots successfully.
Has completed its initial configuration.
Has working internet access.
Has the required network settings.

Shut down Windows normally.

Open the Snapshots view in VirtualBox and create a snapshot named:

clean-install

Add a description:

Clean Windows 11 Enterprise Evaluation installation with initial configuration and network verification.

Why: A separate snapshot protects the Windows guest independently from Ubuntu.

Screenshot: evidence/24-windows-clean-snapshot.png

Step 25. Verify snapshot availability

Return to the VirtualBox main window.

Check that both virtual machines have a snapshot named clean-install.

Do not make destructive changes until both snapshots are available.

Why: Verifying the snapshots before testing prevents the mistake that occurred during my first laboratory, when the snapshot was created after the system had already been modified.

Screenshot: evidence/25-snapshot-list.png

10. Final verification checklist

Before considering the environment complete, verify every requirement.

Requirement	Expected result
Ubuntu VM	Starts successfully
Windows VM	Starts successfully
Ubuntu RAM	2 GB
Windows RAM	4 GB
Ubuntu disk	25 GB maximum
Windows disk	64 GB maximum
Ubuntu internet	Working
Windows internet	Working
Ubuntu to Windows	Successful ping
Windows to Ubuntu	Successful ping
Ubuntu snapshot	clean-install exists
Windows snapshot	clean-install exists

Save the command outputs and screenshots in the evidence/ directory.

11. Troubleshooting
Problem 1. Ubuntu keeps the old IP address

Symptom: After changing the network adapter to NAT Network, Ubuntu still displays its previous IP address.

Possible cause: The guest has not renewed its DHCP lease or its network interface has not been reinitialized.

Solution:

Shut down Ubuntu completely.
Check that Adapter 1 uses DevTestNetwork.
Verify that DHCP is enabled.
Start Ubuntu again.
Run:
bash
ip -br addr


Check whether the guest received an address from the correct subnet.

This problem occurred during the paired networking exercise and demonstrated why a complete shutdown and restart may be necessary after changing network settings.

Problem 2. Windows does not respond to ping

Symptom: Ubuntu cannot receive ping replies from Windows.

Possible cause: Windows Defender Firewall blocks incoming ICMPv4 echo requests.

Solution:

Check the firewall configuration and create the specific inbound ICMPv4 rule described in Step 22.

Do not disable Windows Defender Firewall completely.

Problem 3. The host becomes slow

Symptom: Windows host performance decreases when both virtual machines are running.

Possible cause: Too much RAM has been allocated to the guests or other host applications are using available memory.

Solution:

Shut down the virtual machines if the host becomes unresponsive.
Check memory usage in Windows Task Manager.
Verify the configured RAM allocations.
Avoid allocating more memory than the host can comfortably provide.

During the paired laboratory exercise, allocating 10 GB to each guest on a 16 GB host caused severe performance problems.

The configuration in this document uses a more conservative total allocation of 6 GB.

12. Expected final result

After completing all steps, the host computer should contain two independent virtual machines:

lab01-ubuntu — Ubuntu Server 26.04.1 LTS.
lab02-windows — Windows 11 Enterprise Evaluation.

Both guests should use the same NAT Network, access the internet and communicate with each other.

Each guest should also have a verified clean-install snapshot.

The resulting environment provides a reusable testing setup for basic development tasks while keeping the guest operating systems separate from the host computer.

The evidence directory should contain the screenshots and command outputs collected during the actual build and verification process.
