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

The requester needs a test environment for a new developer and documentation that another employee can follow to recreate the same environment independently.

2. What was actually needed

The original request does not provide enough technical information to build a reliable testing environment.

Information missing from the request:

The host computer's hardware specifications and available disk space.
Which Linux distribution and Windows version should be installed.
Whether both operating systems need to run simultaneously.
Whether both guests need internet access.
Whether the guests must communicate with each other.
Which virtualization platform should be used.
How the environment should be restored after unsuccessful experiments.
What software and workloads the new developer will need to test.

Where the request was wrong:

The phrase "without breaking anything" suggests that simply creating a virtual machine guarantees complete protection. This is not correct. Virtual machines provide separation from the host, but incorrect networking, shared folders, resource allocation and configuration can still create risks.

The request also describes a single test computer, while the actual assignment requires two separate guest operating systems: Linux and Windows. A single guest would not provide the required cross-platform testing environment.

What I assumed instead of asking:

I would have asked Aigerim about the developer's software requirements, Windows licensing, expected workloads, internet access and whether the two guests need to communicate.

Since these details were not provided, I used the following assumptions:

The host is a Windows computer with 16 GB RAM and 6 CPU cores.
Oracle VirtualBox is the virtualization platform.
Ubuntu Server 26.04.1 LTS is the Linux guest.
Windows 11 Enterprise Evaluation is the Windows guest.
Both guests require internet access and communication with each other.
Snapshots are required to restore a clean working state.
The environment should support basic development and testing activities.

The Windows configuration is based on the configuration used during the paired Lab 2 exercise. That exercise was performed on my partner's computer, so I use its results as a technical reference, not as proof of a successful build on my own host.

Networking decision:

I selected a shared NAT Network as the intended configuration.

I considered three alternatives:

Standard NAT: Provides internet access, but separate standard NAT adapters do not normally allow direct communication between guests.
Bridged networking: Allows guests to appear as separate devices on the physical network, but university Wi-Fi may restrict additional MAC addresses.
Host-Only networking: Allows private communication between guests but does not provide internet access by itself.

NAT Network offers the best balance for this project because both guests can communicate while retaining internet connectivity through the host.

3. What I did
3.1 Host inventory

I used the results of Lab 1 to identify the host hardware.

Resource	Host specification
Operating system	Windows
RAM	16 GB
CPU cores	6
Hypervisor	Oracle VirtualBox

The available physical disk space has not yet been independently recorded and must be checked before finalizing the installation.

3.2 Linux virtual machine

The existing Linux VM was created during Lab 1 on my own computer.

Setting	Configuration
VM name	lab01-ubuntu
Operating system	Ubuntu Server 26.04.1 LTS
CPU	1 vCPU
RAM	2 GB
Virtual disk	25 GB
Disk allocation	Dynamically allocated
Network	NAT in the original Lab 1 configuration

The Ubuntu VM was tested by booting it, checking system information, intentionally removing /etc/hostname, and restoring the machine from a snapshot.

The installation and recovery process provided practical experience with VM snapshots and system restoration.

3.3 Windows virtual machine

The Windows configuration used as a reference comes from Lab 2, which was performed on my partner's computer.

Setting	Reference configuration
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

The Windows installation media is available from Microsoft's official evaluation centre:

Windows 11 Enterprise Evaluation

This configuration is a proposed target for my own host and must be verified on that machine before it can be considered completed.

3.4 Resource allocation

The host has 16 GB RAM and 6 CPU cores.

The proposed simultaneous allocation is:

Component	RAM	CPU
Windows host (remaining allocation)	10 GB	3 cores
Ubuntu guest	2 GB	1 vCPU
Windows guest	4 GB	2 vCPU
Total	16 GB	6 cores

The guests receive 6 GB of RAM in total, leaving approximately 10 GB for the host before accounting for other applications and virtualization overhead.

This allocation is more conservative than the 10 GB + 10 GB overcommitment tested during Lab 2.

That configuration caused severe host memory pressure, slow performance and temporary mouse freezes. Therefore, I chose 2 GB for Ubuntu and 4 GB for Windows instead of assigning 10 GB to each guest.

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

4.1 Required Ticket 1 evidence
File	What it proves	Status
01-new-vm-wizard.png	Shows VM creation settings and operating system selection.	To collect
02-disk-settings.png	Shows virtual disk size and dynamic allocation.	To collect
03-network-adapter.png	Shows both guests configured on the same NAT Network.	To collect
04-guest-uname.txt	Shows Linux kernel and operating system information.	To collect
05-ping-between-guests.txt	Shows internet connectivity and successful communication between Ubuntu and Windows.	To collect
06-snapshot-list.png	Shows clean snapshots for both virtual machines.	To collect
4.2 Existing Lab 1 evidence

The following evidence was collected on my own computer during Lab 1.

labs/lab01-first-vm/evidence/vm-settings.png — proves the Ubuntu VM's original CPU, memory and disk configuration.
labs/lab01-first-vm/evidence/guest-release.png — proves the installed Ubuntu release.
labs/lab01-first-vm/evidence/broken.png — shows the intentionally broken Ubuntu state.
labs/lab01-first-vm/evidence/snapshot-list.png — shows the snapshot list from Lab 1.
labs/lab01-first-vm/evidence/restored-after-rollback.png — shows the restored Ubuntu guest.
4.3 Existing Lab 2 reference evidence

The following evidence was produced during a paired laboratory exercise on my partner's computer.

labs/lab02-windows-vm/evidence/ipconfig-windows.txt — shows the Windows guest IP configuration.
labs/lab02-windows-vm/evidence/ip-addr-linux.txt — shows the Linux guest IP configuration.
labs/lab02-windows-vm/evidence/ping-both-ways.txt — records network connectivity tests.
labs/lab02-windows-vm/evidence/host-memory-before.png — shows host memory usage before running both guests.
labs/lab02-windows-vm/evidence/host-memory-after.png — shows host memory pressure after running both guests.

These files are reference material and must not be treated as evidence of a successful build on my own host.

5. What I would do differently

I would verify the host's available disk space and memory pressure before deciding on the final VM allocation. Lab 2 demonstrated that excessive memory allocation can make the host almost unusable, even when the guests themselves have enough memory.

I would also create and verify clean snapshots immediately after installation rather than after beginning experiments. In Lab 1, the snapshot timing error prevented recovery to the intended original state.

Finally, I would test the complete BUILD.md instructions on another student's computer before submission. This would reveal unclear steps and missing information that might not be obvious to the original author.

Submission status: Documentation prepared. Windows VM creation on my own host, final network verification, Ticket 1 evidence collection and peer review are still required.
