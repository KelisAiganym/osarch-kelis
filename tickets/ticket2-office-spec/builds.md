Ticket 2 — Office Computer Builds
Design principles

The four computers are designed for four different workloads. An identical configuration would waste money on resources that some employees do not need and underfund resources that other employees depend on.

The total procurement budget is 3,000,000 KZT, including monitors and licensed operating systems.

The design uses AM4/DDR4 for the two general office systems to control costs, and AM5/DDR5 for the developer workstation to provide a stronger upgrade path. The designer receives a dedicated NVIDIA GPU.

Build 1 — Accountant

Load: The accountant works approximately 7–8 hours per day with accounting software, Excel spreadsheets, PDF documents, email and online banking. Several office applications are open simultaneously. The expected workload is mostly light CPU processing, with occasional larger spreadsheets.

Bottleneck: Memory capacity and reliability are more important than graphics performance. A 16 GB configuration is the minimum acceptable starting point. The integrated Radeon graphics avoids spending money on a separate GPU.

Trade-off: I selected an AMD Ryzen 5 5600G instead of a more expensive Ryzen 7. I accepted the older AM4/DDR4 platform because the workload does not require a high-end CPU. The money is better spent on a 1 TB NVMe SSD, a monitor and a licensed Windows installation.

Configuration:

CPU: AMD Ryzen 5 5600G
Motherboard: MSI B550M PRO-VDH
Memory: 16 GB DDR4-3200, dual channel
Storage: Kingston NV3 1 TB M.2 NVMe
Graphics: Integrated Radeon Vega 7
PSU: Deepcool PF550, 550 W
Case: PCcooler C3 B310
CPU cooler: Deepcool AG400
Monitor: Xiaomi A24i, 24-inch IPS
Operating system: Windows 11 Pro OEM

Compatibility: The processor and motherboard use the AM4 socket. The board supports DDR4 memory and has an M.2 slot for the NVMe SSD. The integrated GPU provides display output without a dedicated graphics card.

Total: 447,479 KZT

Build 2 — Sales Manager

Load: The sales manager works approximately 8 hours daily with a CRM application, 30–40 browser tabs, Excel, email, messaging applications and several hours of video calls. Multiple applications must remain responsive at the same time.

Bottleneck: RAM is the main bottleneck. Browser tabs and video conferencing consume memory simultaneously. With only 16 GB, the system may start swapping when the workload grows.

Trade-off: I kept the same affordable Ryzen 5 5600G processor but increased memory to 32 GB. I did not purchase a dedicated GPU because CRM, spreadsheets and video conferencing do not justify its cost. The saved money goes to memory capacity rather than graphics performance.

Configuration:

CPU: AMD Ryzen 5 5600G
Motherboard: MSI B550M PRO-VDH
Memory: 32 GB DDR4-3200, 2 × 16 GB
Storage: Kingston NV3 1 TB M.2 NVMe
Graphics: Integrated Radeon Vega 7
PSU: Deepcool PF550, 550 W
Case: PCcooler C3 B310
CPU cooler: Deepcool AG400
Monitor: Xiaomi A24i, 24-inch IPS
Operating system: Windows 11 Pro OEM

Compatibility: AM4 CPU, B550 motherboard and DDR4 memory are compatible. The board's M.2 slot supports the NVMe drive. The integrated graphics is sufficient for the stated workload.

Total: 517,757 KZT

Build 3 — Web Developer

Load: The developer works 8 hours per day using VS Code, Docker containers, local databases, multiple browser windows, terminals, Git and potentially virtual machines. Several development services may run simultaneously.

Bottleneck: RAM capacity and CPU multithreaded performance. Containers and virtual machines reserve memory even when they are not actively used. Running several services alongside an IDE and browsers can exceed 16 GB.

Trade-off: I allocated a larger budget to the CPU and chose the AM5 platform with DDR5 memory. I used integrated graphics instead of a discrete GPU because software development does not normally require dedicated graphics acceleration. I chose 32 GB as the initial configuration rather than spending more on 64 GB immediately. The motherboard provides a future memory upgrade path.

Configuration:

CPU: AMD Ryzen 7 7700 BOX
Motherboard: Gigabyte B650M D3HP
Memory: 32 GB DDR5-5600
Storage: Kingston NV3 1 TB M.2 NVMe
Graphics: Integrated Radeon Graphics
PSU: Deepcool PF650, 650 W
Case: PCcooler C3 B310
CPU cooling: Included BOX cooler
Monitor: Xiaomi A24i, 24-inch IPS
Operating system: Ubuntu 24.04 LTS

Compatibility: Ryzen 7 7700 uses AM5 and supports DDR5. The B650 motherboard supports AM5 processors, DDR5 memory and M.2 NVMe storage. The processor includes integrated graphics and a stock cooler. The 650 W PSU provides sufficient power capacity for this configuration.

Operating system justification: Ubuntu is a free and open-source operating system distributed under open-source licences. It is suitable for programming, Docker, Git, Linux development tools and local databases. The organisation does not need to purchase a Windows licence for this workstation, provided the developer's required applications support Linux.

Total: 774,055 KZT

Build 4 — Designer

Load: The designer works approximately 8 hours daily with Photoshop, Illustrator, Figma and large image files. The workload includes multiple applications, large documents, high-resolution images and colour-sensitive visual work.

Bottleneck: GPU memory and system RAM can become limiting during complex image processing. Monitor resolution and colour reproduction are also important because the employee must inspect visual details accurately.

Trade-off: I chose an NVIDIA RTX 5060 8 GB rather than a more expensive professional GPU. I selected 32 GB DDR4 memory instead of 16 GB because creative applications need room for large files. I spent more on a 27-inch QHD IPS monitor instead of a basic 24-inch display. I accepted the older AM4 platform to reserve enough budget for graphics and display quality.

Configuration:

CPU: AMD Ryzen 5 5600
Motherboard: MSI B550M PRO-VDH
Memory: 32 GB DDR4-3200, 2 × 16 GB
Storage: Kingston NV3 1 TB M.2 NVMe
GPU: NVIDIA GeForce RTX 5060 8 GB
PSU: Deepcool PF650, 650 W
Case: PCcooler C3 B310
CPU cooler: Deepcool AG400
Monitor: Xiaomi G27Qi, 27-inch QHD IPS
Operating system: Windows 11 Pro OEM

Compatibility: The Ryzen 5 5600 uses the AM4 socket, and the B550 motherboard supports DDR4 memory and the NVMe drive. The RTX 5060 uses a PCIe graphics slot. The 650 W PSU provides suitable power capacity for the CPU and GPU combination.

Total: 883,880 KZT

Final budget summary
Role	Total
Accountant	447,479 KZT
Sales	517,757 KZT
Web Developer	774,055 KZT
Designer	883,880 KZT
Grand total	2,623,171 KZT
Remaining budget	376,829 KZT

The project stays within the 3,000,000 KZT budget.

The remaining money is a reserve for price changes, delivery, assembly and possible future memory upgrades. It should not be treated as proof that these additional services have already been purchased.

Upgrade strategy

The first upgrade should be the web developer's RAM from 32 GB to 64 GB. This will help when Docker containers, local databases and virtual machines run simultaneously.

The designer's next upgrade should be a GPU with more than 8 GB of VRAM if the workload expands to complex video editing or 3D work.

The accountant and sales computers can remain on their initial configurations until actual workload monitoring shows a need for more memory.
