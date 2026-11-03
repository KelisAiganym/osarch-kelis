Ticket 1 — VM Build Document

Student: Kelis Aiganym
Host Operating System: Windows
Hypervisor: Oracle VirtualBox
Linux Guest: Ubuntu Server 26.04.1 LTS
Windows Guest: Windows 11 Enterprise Evaluation

1. What Was Asked

Original request

From: Aigerim T. (Office Manager)
Subject: test machine for new dev

Hi, we have someone starting on Monday and he needs a test computer set up. Nothing special, just so he can try things without breaking anything. Can you write down how to do it so I can do the next one myself? Thanks

The requester needs a safe testing environment for a new developer and documentation that another employee can follow to recreate it.

2. What Was Actually Needed

The original request does not contain enough technical information to build a complete and reliable testing environment.

Information missing from the request:

Available RAM, CPU resources and physical disk space on the host.
Required Linux distribution and Windows version.
Whether both operating systems must run simultaneously.
Whether both guests require internet access.
Whether the guests must communicate with each other.
Which virtualization platform should be used.
How the environment should be restored after unsuccessful experiments.
What software the developer will need to test.

Where the request was incorrect:

The requester describes one test computer, but the assignment requires two separate guest operating systems: Linux and Windows. One guest alone would not provide the required cross-platform testing environment.

The phrase "without breaking anything" also creates an incorrect expectation. Virtual machines provide isolation, but they do not guarantee complete protection against incorrect configuration, unsafe shared folders or resource exhaustion.

Questions I would have asked:

I would have asked about the developer's software requirements, available disk space, Windows licensing, expected workloads and network requirements.

Assumptions made instead of answers:

Missing information	Assumption
Host specifications	Windows host with 16 GB RAM and 6 CPU cores, based on Lab 1
Hypervisor	Oracle VirtualBox
Linux system	Ubuntu Server 26.04.1 LTS
Windows system	Windows 11 Enterprise Evaluation
Internet access	Required for both guests
Guest communication	Required between both guests
Recovery	Clean snapshots for both guests
Intended use	Basic development and software testing

The Windows configuration is based on Lab 2, which was performed on my partner's computer. Its results are used as a technical reference, not as proof of installation on my own host.

Networking decision and alternatives:

I selected NAT Network because it provides internet connectivity and communication between the guests.

Standard NAT: Provides internet access but does not normally allow direct communication between separate guests.
Bridged: Allows guests to appear as separate devices on the physical network, but may be restricted by university Wi-Fi.
Host-Only: Allows private communication but does not provide internet access independently.
NAT Network: Provides a practical balance between guest communication and internet connectivity.
3. What I Did
3.1 Host Inventory

The host specifications were recorded during my completed Lab 1.

Resource	Specification
Operating system	Windows
RAM	16 GB
CPU cores	6
Hypervisor	Oracle VirtualBox

Disk space: The available physical disk capacity was not recorded in Lab 1 and must be checked before completing the new installation.

3.2 Linux Virtual Machine

I completed Lab 1 on my own computer and installed Ubuntu Server 26.04.1 LTS.

Existing VM configuration:

Setting	Configuration
VM name	lab01-ubuntu
Operating system	Ubuntu Server 26.04.1 LTS
CPU	1 vCPU
RAM	2 GB
Virtual disk	25 GB
Disk allocation	Dynamically allocated
Network	NAT

I verified that Ubuntu booted successfully, checked the operating system information, intentionally removed /etc/hostname and restored the system using a snapshot.

This exercise demonstrated virtual machine recovery and the importance of creating snapshots before making system changes.

3.3 Windows Virtual Machine

The Windows configuration comes from Lab 2, completed as a paired exercise on my partner's computer.

Setting	Configuration
VM name	lab02-windows
Operating system	Windows 11 Enterprise Evaluation
CPU	2 vCPU
RAM	4 GB
Virtual disk	64 GB
Disk allocation	Dynamically allocated
Firmware	UEFI
Secure Boot	Enabled
TPM	Virtual TPM
Network	NAT Network

Official installation source:
https://www.microsoft.com/en-us/evalcenter/download-windows-11-enterprise

This configuration is used as a reference for the Windows guest build.

3.4 Resource Allocation

The host has 16 GB RAM and 6 CPU cores.

Planned allocation:

Component	RAM	CPU
Windows host	10 GB remaining	3 cores remaining
Ubuntu guest	2 GB	1 vCPU
Windows guest	4 GB	2 vCPU
Total	16 GB	6 cores

The guests receive 6 GB RAM in total, leaving approximately 10 GB for the host before additional application usage and virtualization overhead.

During Lab 2, assigning 10 GB RAM to each guest on a 16 GB host caused severe memory pressure. The computer became very slow, the mouse temporarily froze and opening Task Manager took approximately one minute.

I therefore selected a more conservative allocation of 2 GB for Ubuntu and 4 GB for Windows.

Both virtual disks use dynamic allocation to avoid reserving their maximum capacity immediately.

3.5 Networking

The intended network configuration uses one shared VirtualBox NAT Network.

Both guests must use the same network profile.

During Lab 2, Ubuntu initially retained its old IP address after changing the network adapter. Completely shutting down and restarting Ubuntu allowed it to obtain a new DHCP address.

Windows Defender Firewall also blocked incoming ICMP requests. The problem was resolved during Lab 2 by creating an inbound ICMPv4 firewall rule.

These experiences demonstrate why network connectivity must be tested instead of assumed.

3.6 Snapshots and Recovery

Snapshots provide a recovery point before potentially destructive testing.

During Lab 1, I discovered that a snapshot created after modifying the system could not restore the original clean state.

For this reason, the clean snapshot must be created immediately after installation and initial configuration, before experimental changes.

A separate clean snapshot is required for each guest.

4. Evidence

The following evidence files are included in the Ticket 1 folder.

File	Purpose
evidence/04-guest-uname.txt	Records Ubuntu kernel and operating system information.
evidence/05-ping-between-guests.txt	Records internet and guest-to-guest connectivity tests.

Additional evidence from Lab 1 — my own computer:

File	What it demonstrates
labs/lab01-first-vm/evidence/vm-settings.png	Ubuntu VM resource configuration.
labs/lab01-first-vm/evidence/guest-release.png	Installed Ubuntu release.
labs/lab01-first-vm/evidence/broken.png	Intentionally modified Ubuntu state.
labs/lab01-first-vm/evidence/snapshot-list.png	Existing snapshot configuration.
labs/lab01-first-vm/evidence/restored-after-rollback.png	Ubuntu recovery after snapshot rollback.

Reference evidence from Lab 2 — partner's computer:

File	What it demonstrates
labs/lab02-windows-vm/evidence/ipconfig-windows.txt	Windows IP configuration.
labs/lab02-windows-vm/evidence/ip-addr-linux.txt	Linux IP configuration.
labs/lab02-windows-vm/evidence/ping-both-ways.txt	Guest communication tests.
labs/lab02-windows-vm/evidence/host-memory-before.png	Host memory before running both guests.
labs/lab02-windows-vm/evidence/host-memory-after.png	Host memory pressure after running both guests.

Evidence distinction: Lab 1 files are evidence from my own computer. Lab 2 files are reference material from my partner's computer and are not presented as my own execution results.

5. What I Would Do Differently

I would measure available disk space and actual memory usage before finalizing resource allocation. Lab 2 demonstrated that excessive memory allocation can make the host almost unusable.

I would also create and verify clean snapshots immediately after installation. In Lab 1, incorrect snapshot timing prevented restoration to the intended original state.

Finally, I would ask another student to follow BUILD.md before submission. This would help identify unclear instructions and missing steps that are difficult for the original author to notice.

Current status: Documentation prepared. Additional VM verification, evidence collection and peer review are required before final submission.
