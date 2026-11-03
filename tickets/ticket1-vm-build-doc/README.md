
# Ticket 1 — VM Build Document

**Course:** Operating System Architecture  
**Student:** Kelis Aiganym  
**Repository:** osarch-kelis  
**Host operating system:** Windows  
**Host RAM:** 16 GB  
**Host CPU:** 6 cores  
**Hypervisor:** Oracle VirtualBox  
**Guest operating systems:** Ubuntu Server 26.04.1 LTS and Windows 11 Enterprise Evaluation

---

## 1. What was asked

**Original request:**

From: Aigerim T. (Office Manager)  
Subject: test machine for new dev

> Hi, we have someone starting on Monday and he needs a test computer set up. Nothing special, just so he can try things without breaking anything. Can you write down how to do it so I can do the next one myself? Thanks

The requester wants a safe testing environment for a new developer and documentation that another employee can follow to create the same environment independently.

---

## 2. What was actually needed

The request is incomplete because it does not specify the technical requirements needed to build a working and reproducible testing environment.

### Missing information

The request does not specify:

- The host computer's available RAM, CPU resources and disk space.
- Which Linux distribution and Windows version are required.
- Whether both operating systems must run simultaneously.
- Whether the virtual machines need internet access.
- Whether the virtual machines must communicate with each other.
- Which hypervisor should be used.
- How the developer should restore a clean system after unsuccessful experiments.
- Which development tools and applications the new employee will use.

### Where the request was wrong

The requester assumes that creating a virtual machine automatically guarantees that nothing can be damaged. This is not completely correct. Virtualization provides isolation, but shared folders, networking, incorrect resource allocation and configuration mistakes can still create risks.

The request also mentions one test computer, while the actual assignment requires two guest operating systems: Linux and Windows. A single virtual machine would not provide the required cross-platform testing environment.

### Questions I would have asked

Before starting, I would have asked:

1. Which operating systems and applications does the developer need?
2. Does the developer need to run both virtual machines simultaneously?
3. Do both guests need internet access and communication with each other?
4. How much disk space is available on the host?
5. Does the company have a Windows licence, or should an evaluation version be used?

### Assumptions made instead of answers

Since the requester did not provide these details, I made the following assumptions:

- The host is my Windows computer with 16 GB RAM and 6 CPU cores.
- Oracle VirtualBox is the hypervisor.
- Ubuntu Server 26.04.1 LTS is the Linux guest.
- Windows 11 Enterprise Evaluation is the Windows guest.
- Both guests need internet access.
- Both guests need to communicate with each other.
- Snapshots are required to recover from unsuccessful experiments.
- The environment should support basic development and testing activities.

The Ubuntu configuration is based on my completed Lab 1 on my own computer.

The Windows configuration is based on Lab 2, which I completed with my partner on her computer. Therefore, the Windows configuration is a reference for this build, not evidence that Windows has already been installed on my own host.

### Networking decision

I selected a shared NAT Network as the intended network configuration.

I considered three alternatives:

- **Standard NAT:** Provides internet access, but separate standard NAT adapters do not normally allow direct communication between guests.
- **Bridged networking:** Allows guests to appear as separate devices on the physical network, but university Wi-Fi may restrict additional MAC addresses.
- **Host-Only networking:** Allows private communication between guests but does not provide internet access by itself.

I chose NAT Network because it provides a practical balance between internet connectivity and communication between the two virtual machines.

---

## 3. What I did

### 3.1 Host inventory

I used the results of Lab 1 to identify the resources available on my own computer.

| Resource | Specification |
|---|---|
| Host operating system | Windows |
| RAM | 16 GB |
| CPU cores | 6 |
| Hypervisor | Oracle VirtualBox |

The available physical disk space must be checked before completing the Windows installation.

### 3.2 Existing Linux virtual machine

I created my Ubuntu virtual machine during Lab 1.

| Setting | Configuration |
|---|---|
| VM name | lab01-ubuntu |
| Operating system | Ubuntu Server 26.04.1 LTS |
| CPU | 1 vCPU |
| RAM | 2 GB |
| Virtual disk | 25 GB |
| Disk allocation | Dynamically allocated |
| Original network mode | NAT |

The Ubuntu VM was successfully installed and booted.

During Lab 1, I intentionally deleted the `/etc/hostname` file, restarted the VM and restored it using a snapshot.

This exercise demonstrated the importance of snapshots and recovery points.

For Ticket 1, the existing Ubuntu installation will be reused instead of unnecessarily creating another identical Linux VM.

Its network configuration must be checked and changed to the shared NAT Network if required.

### 3.3 Windows virtual machine

The Windows configuration is based on my completed paired Lab 2 exercise.

| Setting | Reference configuration |
|---|---|
| VM name | lab02-windows |
| Operating system | Windows 11 Enterprise Evaluation |
| CPU | 2 vCPU |
| RAM | 4 GB |
| Virtual disk | 64 GB |
| Disk allocation | Dynamically allocated |
| Firmware | UEFI |
| Secure Boot | Enabled |
| TPM | Virtual TPM 2.0 |
| Intended network | Shared NAT Network |

Windows installation media:

[Windows 11 Enterprise Evaluation](https://www.microsoft.com/en-us/evalcenter/download-windows-11-enterprise)

This VM must be created and tested on my own computer before the build can be considered complete.

### 3.4 Resource allocation

My computer has 16 GB RAM and 6 CPU cores.

The proposed allocation for running both guests simultaneously is:

| Component | RAM | CPU |
|---|---:|---:|
| Windows host | 10 GB remaining | 3 cores remaining |
| Ubuntu guest | 2 GB | 1 vCPU |
| Windows guest | 4 GB | 2 vCPU |
| **Total** | **16 GB** | **6 cores** |

The virtual machines use 6 GB of RAM together, leaving approximately 10 GB for Windows and other applications.

This allocation was chosen after considering the results of Lab 2.

During Lab 2, my partner and I tested an allocation of 10 GB RAM for each guest on a 16 GB host. The host became extremely slow, available memory dropped to approximately 73 MB, and the mouse froze periodically.

I therefore chose a more conservative allocation instead of repeating the previous overcommitment.

The virtual disks are dynamically allocated to avoid reserving their maximum capacity immediately. However, the host must still have enough physical disk space for the actual growth of both virtual disks.

### 3.5 Networking

The intended configuration is one shared VirtualBox NAT Network.

Both virtual machines must be connected to the same NAT Network.

This configuration allows the guests to communicate with each other while accessing the internet through the host.

The following commands will be used for verification:

**Ubuntu:**

```bash
ip addr
ip route
ping -c 4 8.8.8.8
```

**Windows:**

```powershell
ipconfig
ping 8.8.8.8
```

Communication between the guests will also be tested using their actual IP addresses.

During Lab 2, an initial communication problem occurred because Ubuntu retained its previous network configuration after the adapter was changed. Completely shutting down and restarting the VM allowed it to obtain a new DHCP address.

Another issue occurred because Windows Defender Firewall blocked incoming ICMP requests. During Lab 2, this was resolved by creating an inbound ICMPv4 firewall rule.

These problems demonstrate why network connectivity must be tested rather than assumed.

### 3.6 Snapshots and recovery

Snapshots provide recovery points before potentially destructive testing.

During Lab 1, I discovered that a snapshot created after modifying the system could not restore the original clean state.

For Ticket 1, each guest should have a clean snapshot created immediately after installation and initial configuration.

The snapshots should be named clearly:

- `ubuntu-clean-install`
- `windows-clean-install`

This makes it easier for another employee to identify and restore the correct system state.

---

## 4. Evidence

Evidence must demonstrate the actual configuration and successful operation of the virtual machines.

### 4.1 Ticket 1 evidence

| File | What it proves | Status |
|---|---|---|
| `01-new-vm-wizard.png` | Shows the Windows VM creation wizard and selected operating system. | Pending |
| `02-disk-settings.png` | Shows virtual disk size and dynamic allocation. | Pending |
| `03-network-adapter.png` | Shows both guests connected to the same NAT Network. | Pending |
| `04-guest-uname.txt` | Shows Ubuntu kernel and operating system information. | Pending |
| `05-ping-between-guests.txt` | Shows internet connectivity and communication between Ubuntu and Windows. | Pending |
| `06-snapshot-list.png` | Shows clean snapshots for both virtual machines. | Pending |

### 4.2 Existing Lab 1 evidence

The following evidence was collected on my own computer.

- `labs/lab01-first-vm/evidence/vm-settings.png` — proves the original Ubuntu VM CPU, memory and disk settings.
- `labs/lab01-first-vm/evidence/guest-release.png` — proves the installed Ubuntu Server version.
- `labs/lab01-first-vm/evidence/broken.png` — shows the intentionally broken Ubuntu state.
- `labs/lab01-first-vm/evidence/snapshot-list.png` — shows the snapshot list from Lab 1.
- `labs/lab01-first-vm/evidence/restored-after-rollback.png` — shows the restored Ubuntu guest.

### 4.3 Existing Lab 2 reference evidence

The following evidence was collected during a paired exercise on my partner's computer.

- `labs/lab02-windows-vm/evidence/ipconfig-windows.txt` — shows the Windows guest IP configuration.
- `labs/lab02-windows-vm/evidence/ip-addr-linux.txt` — shows the Linux guest IP configuration.
- `labs/lab02-windows-vm/evidence/ping-both-ways.txt` — records network connectivity tests.
- `labs/lab02-windows-vm/evidence/host-memory-before.png` — shows host memory usage before running both guests.
- `labs/lab02-windows-vm/evidence/host-memory-after.png` — shows memory pressure after running both guests.

These files are reference material only and are not presented as evidence of a successful build on my own computer.

---

## 5. What I would do differently

I would check the host's available disk space and memory pressure before starting the installation. Lab 2 showed that allocating too much memory to virtual machines can make the host almost unusable.

I would also create and verify clean snapshots immediately after installation. In Lab 1, the snapshot timing error prevented recovery to the intended original state.

Finally, I would ask another student to follow BUILD.md without additional explanations. This would help identify unclear instructions and missing steps before the document is used by an employee who has never worked with a hypervisor.

---

**Submission status:** The Ubuntu VM and its previous laboratory evidence are available. The Windows VM still needs to be created and verified on my own host. Ticket-specific evidence and peer review are pending.
