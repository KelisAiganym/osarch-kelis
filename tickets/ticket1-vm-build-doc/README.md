Ticket 1 — VM Build Document

Student: Kelis Aiganym
Host Operating System: Windows
Hypervisor: Oracle VirtualBox
Linux Guest: Ubuntu Server 26.04.1 LTS
Windows Guest: Windows 11 Enterprise Evaluation

1. What Was Asked
1.1 Original Support Request

From: Aigerim T. (Office Manager)
Subject: test machine for new dev

Hi, we have someone starting on Monday and he needs a test computer set up. Nothing special, just so he can try things without breaking anything. Can you write down how to do it so I can do the next one myself? Thanks

The requester needs a safe testing environment for a new developer and instructions that another employee can follow to recreate the same environment independently.

The expected result is a repeatable virtual machine setup that allows software testing without directly modifying the host operating system.

2. What Was Actually Needed

The original request is incomplete because it describes the desired result without specifying the technical requirements necessary to build and reproduce the environment.

2.1 Information Missing from the Request

The request does not specify:

Host hardware: Available RAM, CPU cores and free physical disk space.
Operating systems: Which Linux distribution and Windows version are required.
Virtualization platform: Whether VirtualBox, VMware or another hypervisor should be used.
Networking: Whether the guests need internet access and communication with each other.
Resource allocation: Whether both virtual machines must run simultaneously.
Recovery: How to restore a guest after unsuccessful experiments.
Windows licensing: Whether an evaluation version is acceptable.
Developer workload: Which applications, programming tools and services must be tested.
Security: Whether shared folders, clipboard sharing and other host integration features are required.
2.2 Where the Request Was Incorrect

The requester describes a single test computer. However, the actual assignment requires two separate guest operating systems: Linux and Windows.

A single guest would not satisfy the cross-platform testing requirement.

The phrase "without breaking anything" is also misleading. Virtual machines provide isolation from the host, but they do not guarantee complete protection. Incorrect network settings, unsafe shared folders, excessive resource allocation and destructive operations can still cause problems.

Therefore, the actual task is to create a documented, isolated and recoverable environment with two different operating systems, rather than simply create one virtual machine.

2.3 Questions I Would Have Asked

Before starting, I would have asked the requester:

Which development tools and applications will the new employee use?
Does the employee need both Linux and Windows?
Must both virtual machines run simultaneously?
Do both guests need internet access?
Must the guests communicate with each other?
How much physical disk space is available?
Is Windows Enterprise Evaluation sufficient for the intended testing period?
2.4 Assumptions Made Instead of Answers

Since the request did not contain these details, I made the following assumptions.

Missing information	Assumption used
Host hardware	Windows host with 16 GB RAM and 6 CPU cores, based on my completed Lab 1
Hypervisor	Oracle VirtualBox
Linux operating system	Ubuntu Server 26.04.1 LTS
Windows operating system	Windows 11 Enterprise Evaluation
Internet access	Required for both guests
Communication between guests	Required
Recovery method	Separate clean snapshots for both guests
Intended workload	Basic development and software testing
Virtual disk type	Dynamically allocated
Network mode	Shared NAT Network

The host specifications are taken from my own Lab 1.

The Windows VM configuration is taken from Lab 2, which was performed on my partner's computer. I use this configuration as a technical reference, not as evidence that Windows has already been installed on my own computer.

2.5 Technical Decisions and Alternatives
Virtualization platform

I selected Oracle VirtualBox because it was already used for my Ubuntu virtual machine in Lab 1. Using the same platform avoids introducing another hypervisor and makes the build process easier to reproduce.

An alternative would be VMware. It provides similar virtualization functionality, but switching platforms would require recreating and documenting the environment.

Network configuration

I selected a shared NAT Network for the intended final configuration.

Network mode	Advantages	Disadvantages
Standard NAT	Provides internet access	Separate guests cannot normally communicate directly
Bridged Adapter	Guests appear as separate devices on the physical network	May be restricted by university Wi-Fi or MAC filtering
Host-Only Adapter	Allows private communication	Does not provide internet access independently
NAT Network	Allows communication between guests and internet access	Requires both guests to use the same NAT Network

NAT Network is the best compromise because it provides internet access and communication between the two guests without requiring them to appear as separate devices on the physical network.

Disk allocation

I selected dynamically allocated virtual disks instead of fixed-size disks.

A dynamic disk uses physical storage as the guest requires it. A fixed-size disk reserves its full capacity immediately.

The trade-off is that dynamic disks can grow over time, so the host must have sufficient free disk space.

Hypervisor Licensing

Oracle VirtualBox is free to use as an open-source virtualization platform under the GNU General Public License (GPL) for its base package. The separate Oracle VirtualBox Extension Pack has different licensing terms and must be considered separately.

For this lab, I use the standard VirtualBox functionality and do not assume that every optional extension is freely licensed for every type of use.

Before deployment in a workplace, the organization should check the applicable VirtualBox and Extension Pack licensing terms.

Designing for a Non-Technical User

The instructions are intended for Aigerim, an office manager who may not have previous experience with virtualization.

Therefore, the build documentation should not assume that the reader understands terms such as NAT Network, virtual disk allocation, EFI, TPM or snapshots.

Each technical step should include:

The exact VirtualBox menu or setting to open.
The value that must be selected.
A screenshot showing the expected configuration.
A short explanation of why the setting is necessary.
A verification step to confirm that the operation succeeded.

This makes the procedure reproducible by a non-IT employee rather than requiring additional technical assistance.

3. What I Did
3.1 Host Inventory

I used the hardware information recorded during my completed Lab 1.

Resource	Specification
Host operating system	Windows
RAM	16 GB
CPU cores	6
Hypervisor	Oracle VirtualBox
Available physical disk space	Not recorded in Lab 1; must be checked before final installation

The host has 16 GB of RAM and 6 CPU cores. These figures provide the basis for allocating resources to the virtual machines.

I did not invent a disk capacity because the available physical disk space was not recorded.

3.2 Existing Linux Virtual Machine

I created and tested the Ubuntu Server VM during Lab 1 on my own computer.

VM Configuration
Setting	Configuration 
VM name	lab01-ubuntu
Operating system	Ubuntu Server 26.04.1 LTS
CPU	1 vCPU
RAM	2 GB
Virtual disk	25 GB
Disk allocation	Dynamically allocated
Network	NAT
Installation and Testing

During Lab 1, I:

Installed Oracle VirtualBox.
Created the lab01-ubuntu virtual machine.
Allocated 1 vCPU and 2 GB RAM.
Created a 25 GB dynamically allocated virtual disk.
Configured NAT Network
Installed Ubuntu Server 26.04.1 LTS.
Booted the guest and checked its operating system information.
Tested system recovery using a snapshot.
Recovery Experiment

I intentionally modified the Ubuntu system by deleting its hostname file.

Commands used:

bash
cat /etc/hostname
sudo rm /etc/hostname
cat /etc/hostname
sudo reboot
hostname


This experiment demonstrated how destructive system changes can affect a guest operating system.

An important problem occurred with snapshot timing. The snapshot named clean-install was created at 18:32, while the hostname file had been deleted at 18:31.

Therefore, the snapshot was not a true pre-change recovery point. This showed that a clean snapshot must be created immediately after installation and initial configuration, before performing destructive experiments.

The existing Lab 1 screenshots document the VM configuration, the modified state and the subsequent recovery attempt.

3.3 Windows Virtual Machine

The Windows configuration is based on the paired Lab 2 exercise performed on my partner's computer.

It is reference information and is not presented as a completed Windows installation on my own host.

Reference Configuration
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
Network	NAT Network in the intended configuration

Official Windows evaluation download:

https://www.microsoft.com/en-us/evalcenter/download-windows-11-enterprise

The evaluation version is suitable for a temporary testing environment. It is not a substitute for a permanent licensed Windows installation.

The Windows VM must be created and verified on my own host before the build can be considered fully reproduced.

3.4 Resource Allocation

The resource allocation is based on my own host specifications: 16 GB RAM and 6 CPU cores.

Planned Resource Allocation
Component	RAM	CPU
Windows host	Approximately 10 GB remaining before other application usage	3 cores remaining
Ubuntu guest	2 GB	1 vCPU
Windows guest	4 GB	2 vCPU
Total guest allocation	6 GB	3 vCPU

The two guests receive 6 GB of RAM in total. This leaves approximately 10 GB for Windows before accounting for other applications and virtualization overhead.

I selected 2 GB for Ubuntu because it is a lightweight server installation without a graphical desktop.

I selected 4 GB for Windows because it is the minimum memory requirement for Windows 11, although additional memory may be necessary for heavier development workloads.

Why I Did Not Allocate More Memory

During Lab 2, my partner tested an overcommitted configuration with 10 GB RAM assigned to each guest on a 16 GB host.

The results were poor:

The host became extremely slow.
The mouse froze periodically.
Opening Task Manager took approximately one minute.
Available physical memory dropped significantly.

This demonstrated that assigning more memory to virtual machines does not automatically improve performance.

I therefore chose a more conservative allocation rather than repeating the overcommitment problem.

Both disks use dynamic allocation to avoid reserving their maximum capacity immediately. However, the actual free disk space must be checked before creating the Windows guest.

3.5 Network Configuration

The intended final configuration uses one shared VirtualBox NAT Network.

Both virtual machines must use the same NAT Network profile.

The expected result is:

Ubuntu can access the internet.
Windows can access the internet.
Ubuntu can communicate with Windows.
Windows can communicate with Ubuntu.
Problems Observed During Lab 2

Two networking problems occurred during the paired exercise.

Problem 1 — Old IP address

After changing Ubuntu's adapter to NAT Network, the guest initially retained its previous IP address.

The problem was resolved by completely shutting down Ubuntu and starting it again. It then received a new DHCP address.

Problem 2 — Windows Firewall

Windows Defender Firewall blocked incoming ICMPv4 echo requests on the Public network profile.

The issue was resolved during Lab 2 by creating an inbound firewall rule for ICMPv4.

These problems demonstrate why network connectivity must be tested rather than assumed.

The Lab 2 results are reference material because the exercise was performed on my partner's machine.

Updated Network Configuration

The original Ubuntu VM was initially configured with standard NAT networking during Lab 1.

For Ticket 1, I subsequently changed the adapter configuration to a shared NAT Network.

I captured updated evidence of the NAT Network configuration.

The final network configuration must be verified by checking that both Ubuntu and Windows use the same NAT Network profile. A configuration screenshot alone does not prove that communication between the guests works.

Network Verification Status

The NAT Network configuration has been updated and documented.

However, successful connectivity has not yet been demonstrated by actual ping output.

The following tests remain necessary:

Ubuntu to Windows using the Windows guest IP address.
Windows to Ubuntu using the Ubuntu guest IP address.
Ubuntu to an external internet address.
Windows to an external internet address.

The results must be recorded with the actual command output. Failed tests must also be documented rather than presented as successful.

3.6 Snapshots and Recovery

Snapshots are necessary because the purpose of the environment is to allow a developer to experiment without permanently damaging the guest installation.

I plan to create a separate clean snapshot for each guest.

The correct sequence is:

Install the operating system.
Complete initial configuration.
Install necessary updates.
Verify that the guest boots correctly.
Create a snapshot named clean-install.
Perform testing only after the snapshot has been created.

The Lab 1 snapshot timing problem showed why this sequence matters.

A snapshot created after a destructive modification cannot restore the system to a state that existed before that modification.

Rollback Procedure — Restoring a Clean Snapshot

If a developer makes unwanted changes or the guest operating system becomes unstable, the virtual machine can be restored to its clean snapshot.

The recovery procedure is:

Shut down the virtual machine completely.
Open Oracle VirtualBox Manager.
Select the virtual machine that needs recovery.
Open the Snapshots view.
Select the snapshot named clean-install.
Choose Restore.
Confirm the restoration. If necessary, save the current state as a separate snapshot before restoring.
Start the virtual machine.
Verify that the operating system boots correctly and that the initial configuration has been restored.

Important: Restoring a snapshot discards changes made after that snapshot, unless they were saved separately. The user should back up important work before performing rollback.

This procedure must be repeated separately for Ubuntu and Windows because each guest has its own snapshots.

4. Evidence

The evidence is divided into files collected on my own computer, files available in the Ticket 1 folder and reference material from my partner's Lab 2.

4.1 Ticket 1 Evidence — My Own Computer
File	What it proves
evidence/04-guest-uname.txt	Records Ubuntu system and kernel information using uname -a and distribution information using lsb_release -a.
evidence/05-ping-between-guests.txt	Intended to record network connectivity tests. Actual ping commands and their output are still required to prove connectivity.
4.2 Existing Lab 1 Evidence — My Own Computer
File	What it proves
labs/lab01-first-vm/evidence/vm-settings.png	Shows the CPU, RAM and virtual disk configuration of my Ubuntu VM.
labs/lab01-first-vm/evidence/guest-release.png	Shows the Ubuntu operating system release.
labs/lab01-first-vm/evidence/broken.png	Shows the intentionally modified Ubuntu system.
labs/lab01-first-vm/evidence/snapshot-list.png	Shows the snapshot configuration and its timing.
labs/lab01-first-vm/evidence/restored-after-rollback.png	Documents the Ubuntu recovery attempt after rollback.
4.3 Existing Lab 2 Evidence — Partner's Computer
File	What it proves
labs/lab02-windows-vm/evidence/ipconfig-windows.txt	Records the Windows guest IP configuration.
labs/lab02-windows-vm/evidence/ip-addr-linux.txt	Records the Linux guest IP configuration.
labs/lab02-windows-vm/evidence/ping-both-ways.txt	Documents the communication tests between the two guests.
labs/lab02-windows-vm/evidence/host-memory-before.png	Shows host memory usage before running both guests.
labs/lab02-windows-vm/evidence/host-memory-after.png	Shows the memory pressure after running both guests.

These Lab 2 files are reference material from my partner's computer. They are not presented as proof of execution on my own host.

4.4 Additional Evidence Required

The following evidence is still necessary for a complete Ticket 1 submission:

Required file	What it must prove
evidence/01-new-vm-wizard.png	The Windows VM creation wizard and selected operating system.
evidence/02-disk-settings.png	The Windows VM disk size and dynamic allocation.
Windows VM installation screenshot	Shows that Windows 11 Enterprise Evaluation is installed and running on my own computer.
Windows network adapter screenshot	Shows that Windows is connected to the same NAT Network as Ubuntu.
Windows guest IP configuration	Records the Windows guest IP address on my own computer.
Actual ping test results	Demonstrates communication between Ubuntu and Windows and internet connectivity.
Windows clean snapshot screenshot	Shows the Windows snapshot named clean-install.
evidence/06-snapshot-list.png	Shows the clean snapshots for both guests.

The NAT Network configuration for Ubuntu has already been updated and documented in 03-network-adapter.png.

The remaining Windows screenshots and connectivity evidence must be collected from the actual VM configuration on my own computer.

Existing Lab 1 and Lab 2 screenshots must not be renamed or presented as new Ticket 1 evidence.
5. What I Would Do Differently

First, I would record the available physical disk space before selecting virtual disk sizes. In Lab 1, I recorded CPU and RAM but did not record the available disk capacity. This leaves an important part of the resource calculation incomplete.

Second, I would create the clean snapshot immediately after installation and initial configuration. During Lab 1, the snapshot was created after the hostname file had already been deleted. This prevented it from serving as the intended original recovery point.

Third, I would test the complete environment with both guests running simultaneously before finalizing the resource allocation. Lab 2 showed that excessive memory allocation can make the host almost unusable.

Finally, I would ask another student to follow the BUILD.md instructions without additional explanations from me. This would reveal unclear steps, missing screenshots and assumptions that an inexperienced user might not understand.

Submission status: The Ubuntu guest and its Lab 1 evidence are available on my own computer. The Windows configuration and networking results from Lab 2 are reference material from my partner's computer. The complete Ticket 1 build still requires local Windows VM verification, final evidence collection and independent peer review.
