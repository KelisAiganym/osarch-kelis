Ticket 1 — VM Build Document

Course: Operating System Architecture
Student: Kelis Aiganym
Repository: osarch-kelis
Host operating system: Windows
Hypervisor: Oracle VirtualBox
Guest operating systems: Ubuntu Server 26.04.1 LTS and Windows 11 Enterprise Evaluation

1. What was asked

Original request:

From: Aigerim T. (Office Manager)
Subject: test machine for new dev

Hi, we have someone starting on Monday and he needs a test computer set up. Nothing special, just so he can try things without breaking anything. Can you write down how to do it so I can do the next one myself? Thanks

The requester needs a test environment for a new developer and documentation that another employee can follow to create the same environment independently.

2. What was actually needed

The original request does not provide enough technical information to build a reliable testing environment.

Several important details are missing:

The host computer's hardware specifications and available disk space.
Which Linux distribution and Windows version should be installed.
Whether both operating systems need to run simultaneously.
Whether both guests need internet access.
Whether the guests must communicate with each other.
How the test environment should be restored after unsuccessful experiments.
Which virtualization platform should be used.

The request also assumes that a single basic test computer is sufficient. However, the actual requirement is better addressed by two separate virtual machines: one Linux guest and one Windows guest. This allows the developer to test software in different operating systems without directly modifying the host operating system.

I interpreted the request as a requirement to build a reusable environment containing Ubuntu Server and Windows 11, running on one Windows host using VirtualBox.

I would have asked Aigerim about the developer's software requirements, available Windows licensing, expected workloads, internet access and whether the two guests need to communicate. I would also have asked how much disk space could be allocated.

Instead of waiting for these answers, I used the following assumptions:

The host is a Windows computer with 16 GB RAM and 6 CPU cores.
VirtualBox is the virtualization platform.
Ubuntu Server 26.04.1 LTS is the Linux guest.
Windows 11 Enterprise Evaluation is the Windows guest.
Both guests require internet access and communication with each other.
Snapshots are required to restore a clean working state.

The Windows VM configuration is based on the configuration successfully used during the paired Lab 2 exercise. That exercise was performed on another student's computer, so its results are reference material rather than proof of execution on my own host.

The most important design decision is to use a shared NAT Network rather than separate standard NAT adapters. Standard NAT gives internet access but does not normally allow direct communication between separate guests. Bridged networking could provide communication, but it may be blocked by university Wi-Fi restrictions. Host-Only networking provides private communication but does not provide internet access by itself. NAT Network provides a practical balance between guest communication and internet connectivity.

3. What I did
3.1 Host inventory

I used the results of Lab 1 to identify the host hardware:

Resource	Host specification
Operating system	Windows
RAM	16 GB
CPU cores	6
Hypervisor	Oracle VirtualBox
3.2 Linux virtual machine

The existing Linux VM was created during Lab 1.

Setting	Configuration
VM name	lab01-ubuntu
Operating system	Ubuntu Server 26.04.1 LTS
CPU	1 vCPU
RAM	2 GB
Virtual disk	25 GB
Disk allocation	Dynamically allocated
Network	NAT in the original Lab 1 configuration

The Ubuntu VM was tested by booting it, checking system information, intentionally removing /etc/hostname, and restoring the machine from a snapshot.

3.3 Windows virtual machine

The Windows configuration used as a reference comes from Lab 2, which was performed on my partner's computer.

Setting	Reference configuration
VM name	lab02-windows
Operating system	Windows 11 Enterprise Evaluation
CPU	2 vCPU
RAM	4 GB
Virtual disk	64 GB, dynamically allocated
Firmware	UEFI
Secure Boot	Enabled
TPM	Virtual TPM
Network	NAT Network

The Windows installation media is available from Microsoft's official evaluation centre:

https://www.microsoft.com/en-us/evalcenter/download-windows-11-enterprise

This configuration is a proposed target for my own host and must be verified on that machine before it can be considered completed.

3.4 Resource allocation

The host has 16 GB RAM and 6 CPU cores.

The proposed simultaneous allocation is:

Component	RAM	CPU
Windows host	10 GB remaining	3 cores remaining
Ubuntu guest	2 GB	1 vCPU
Windows guest	4 GB	2 vCPU
Total guest allocation	6 GB	3 vCPU

The guests receive 6 GB of RAM in total, leaving approximately 10 GB for the host before accounting for other applications and virtualization overhead.

The allocation is more conservative than the 10 GB + 10 GB overcommitment tested during Lab 2. That configuration caused severe host memory pressure, slow performance and temporary mouse freezes. Therefore, I chose 2 GB for Ubuntu and 4 GB for Windows instead of assigning 10 GB to each guest.

Both virtual disks are dynamically allocated. This avoids reserving their maximum sizes immediately, although sufficient physical disk space must still be available.

3.5 Networking

The intended network configuration is a shared VirtualBox NAT Network.

Both guests should use the same NAT Network profile.

This allows the guests to communicate with each other while accessing the internet through the host.

The network must be verified using:

ip addr on Ubuntu.
ipconfig on Windows.
Ping tests between the guests.
Internet connectivity tests from both guests.

During Lab 2, an initial communication failure was resolved by completely shutting down Ubuntu and restarting it after changing the NAT Network configuration. This allowed it to obtain a new DHCP address.

Windows Defender Firewall also blocked incoming ICMP requests on the Public network profile. The issue was resolved during Lab 2 with an inbound ICMPv4 firewall rule.

3.6 Snapshots and recovery

Snapshots provide a recovery point before testing or modifying the guest operating systems.

During Lab 1, I discovered that a snapshot created after a system modification could not restore the original clean state.

For this reason, the clean snapshot must be created immediately after installation and initial configuration, before destructive testing begins.

A separate snapshot should be created for each guest.

4. Evidence

The following evidence is required for the final submission. Existing Lab 1 evidence is identified separately from evidence that must still be collected for Ticket 1.

File	What it proves	Status
evidence/01-new-vm-wizard.png	VM creation settings and operating system selection	To collect
evidence/02-disk-settings.png	Virtual disk size and dynamic allocation	To collect
evidence/03-network-adapter.png	Both guests configured on the same NAT Network	To collect
evidence/04-guest-uname.txt	Ubuntu kernel and operating system information	To collect
evidence/05-ping-between-guests.txt	Successful communication between Ubuntu and Windows and internet connectivity	To collect
evidence/06-snapshot-list.png	Clean snapshots for both virtual machines	To collect

Existing Lab 1 evidence:

labs/lab01-first-vm/evidence/vm-settings.png — proves the Ubuntu VM's original CPU, memory and disk configuration.
labs/lab01-first-vm/evidence/guest-release.png — proves the installed Ubuntu release.
labs/lab01-first-vm/evidence/broken.png — shows the intentionally broken Ubuntu state.
labs/lab01-first-vm/evidence/snapshot-list.png — shows the snapshot list from Lab 1.
labs/lab01-first-vm/evidence/restored-after-rollback.png — shows the restored Ubuntu guest.

Existing Lab 2 evidence was produced on my partner's computer and is used only as reference material:

labs/lab02-windows-vm/evidence/ipconfig-windows.txt
labs/lab02-windows-vm/evidence/ip-addr-linux.txt
labs/lab02-windows-vm/evidence/ping-both-ways.txt
labs/lab02-windows-vm/evidence/host-memory-before.png
labs/lab02-windows-vm/evidence/host-memory-after.png

These files must not be treated as evidence of a successful build on my own host.

5. What I would do differently

I would verify the host's available disk space and memory pressure before deciding on the final VM allocation. Lab 2 demonstrated that excessive memory allocation can make the host almost unusable, even when the guests themselves have enough memory.

I would also create and verify the clean snapshots immediately after installation rather than after beginning experiments. In Lab 1, the snapshot timing error prevented recovery to the intended original state.

Finally, I would test the complete BUILD.md instructions on another student's computer before submission. This would reveal unclear steps and missing information that might not be obvious to the original author.

Submission status: Documentation prepared; additional local verification and peer review are still required.
