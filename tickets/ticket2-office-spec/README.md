Ticket 2 — Kitting out the new office
1. What was asked

The director, Daniyar K., requested four computers for a new office:

Accountant
Sales manager
Web developer
Designer

The total budget is 3,000,000 KZT. Monitors must be included, and the computers should remain useful for several years.

The director also suggested purchasing four identical machines.

2. What was actually needed

Four identical computers are not the best solution because the four employees have different workloads.

The accountant mainly needs reliable office applications, accounting software, spreadsheets and a licensed Windows environment. A dedicated graphics card would not provide meaningful value for this role.

The sales manager needs more RAM because CRM software, browser tabs, spreadsheets and video calls operate simultaneously.

The web developer needs more CPU processing capacity and memory for Docker, virtual machines, databases, programming tools and browsers.

The designer needs a dedicated GPU, sufficient memory and a higher-resolution IPS monitor for creative applications.

The original request also left several important questions unanswered:

Does the budget include keyboards, mice, cables, surge protectors and UPS devices?
Which accounting and design applications will be installed?
Are any applications licensed by the company already?
Does the developer require Windows-specific development tools?
How many years does the director expect the computers to last?
Is the company planning to use a central domain, remote administration or Windows Pro features?

If I could ask Daniyar, I would confirm the exact software, expected workload, warranty requirements and peripheral budget.

For this project, I assumed:

The company already has keyboards, mice and network infrastructure.
Each employee works approximately eight hours per day.
The accountant requires Windows-compatible accounting software.
The designer uses applications that benefit from NVIDIA GPU acceleration.
The developer can use Ubuntu Linux.
The expected useful service life is approximately four years.
The budget includes the computer, monitor and operating system, but not furniture or office networking equipment.

These assumptions should be confirmed before purchasing.

3. What I did

I analysed each employee's workload before selecting hardware. I identified the main bottleneck for each role and allocated money to the components that address that bottleneck.

The accountant received an affordable six-core processor with integrated graphics and 16 GB RAM.

The sales manager received the same processor but 32 GB RAM because multitasking and browser usage require additional memory.

The web developer received an eight-core Ryzen 7 processor, 32 GB DDR5 memory and an AM5 motherboard to provide a stronger platform for future upgrades.

The designer received an NVIDIA RTX 5060, 32 GB RAM and a 27-inch QHD IPS monitor.

The detailed reasoning is available in builds.md.

The itemised prices, sources and dates are available in budget.csv.

Budget result
Workstation	Cost
Accountant	447,479 KZT
Sales	517,757 KZT
Web Developer	774,055 KZT
Designer	883,880 KZT
Total	2,623,171 KZT

The remaining budget is 376,829 KZT.

Director's question 1 — Which machine will be upgraded first?

I would upgrade the web developer's workstation first, approximately eighteen months after deployment.

The first component to upgrade would be RAM, from 32 GB to 64 GB DDR5. Docker containers, local databases, virtual machines and browser processes can consume memory simultaneously. If memory pressure causes swapping, the developer's system will become less responsive even when the CPU is powerful enough.

The AM5 motherboard provides a suitable upgrade path for additional DDR5 memory and a future compatible processor.

Director's question 2 — Pre-built computers versus custom builds

A comparable pre-built gaming desktop listed on Kaspi costs 851,700 KZT. It includes a Ryzen 5 7600, RTX 4060, 32 GB RAM, a 512 GB SSD and Windows 11 Pro.

However, it does not meet the 1 TB SSD requirement. Adding a 1 TB Kingston SSD at the observed price of 95,708 KZT brings the comparison price to approximately 947,408 KZT per machine.

Four identical pre-built machines would therefore cost approximately 3,789,632 KZT, exceeding the available budget.

The company would gain:

One supplier responsible for the complete system.
Potentially simpler warranty handling.
Less time spent on assembly and compatibility checks.
A ready-to-use configuration.

The company would lose:

Freedom to select individual components.
Better workload-specific allocation of the budget.
Some flexibility in future upgrades.
The ability to avoid paying for unnecessary graphics performance.

This comparison is a benchmark using one pre-built model, not four individually priced role-specific pre-built quotations. A final purchase decision would require four matching vendor quotations.

Director's question 3 — Who will complain first?

The web developer is the most likely person to complain first.

The initial 32 GB RAM configuration may become insufficient when several Docker containers, a local database, multiple browser instances and virtual machines run simultaneously.

The likely complaint would be that the IDE and browser become slow when several development services are running. This is the reason I identified memory as the first planned upgrade.

4. Evidence

The evidence directory contains:

pcpartpicker-links.md — compatibility and power checks for the four configurations.
vendor-quotes/ — dated screenshots of the actual vendor product pages and prices.

The vendor quotations must show the product name, price, vendor and date.

PCPartPicker is used for compatibility and power checks, not for Kazakhstan pricing.

The final prices should be checked against the actual product pages before ordering.

5. What I would do differently

I would perform a two-week workload monitoring period after deployment.

I would record RAM utilisation, CPU utilisation, disk activity and application response times during normal working hours.

This would help identify whether the developer needs 64 GB RAM immediately or whether the initial 32 GB configuration is sufficient.

I would also obtain exact software requirements from the accountant and designer before purchasing. A particular accounting application or design workflow could change the hardware requirements.

Finally, I would consider purchasing UPS devices and a central backup solution to protect business data and improve reliability.

AI assistance declaration

AI assistance was used to help organise the workload analysis, prepare the initial hardware comparison and draft parts of this report. I reviewed the technical decisions and must verify all prices, compatibility, licensing and evidence before submission.
