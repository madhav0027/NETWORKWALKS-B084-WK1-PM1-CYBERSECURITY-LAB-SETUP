🖥️ Cybersecurity & Pentesting Lab Setup

Networkwalks | Project 1 | Tools: VirtualBox + Kali Linux

📌 Project Overview

This project focuses on setting up a virtual cybersecurity and penetration-testing laboratory using VirtualBox and Kali Linux.

The lab provides a controlled environment for practicing security tools, network scanning, and reconnaissance without directly affecting the host operating system or home network.

The environment uses a private VirtualBox NAT Network, allowing additional virtual machines to be added later for authorized security testing.

🎯 Objectives
Install and configure VirtualBox on Linux Mint.
Import Kali Linux as a virtual machine.
Create a private NAT Network for the lab.
Connect Kali Linux to the virtual network.
Assign Kali Linux a static IP address.
Verify network connectivity, DNS resolution, and security-tool installation.
Create a clean VM snapshot for recovery.
Document the setup process, challenges, and solutions.
🛡️ Purpose of the Lab

This virtual lab can be used for:

Network reconnaissance
Port scanning
Vulnerability assessment
Packet analysis
Web application security testing
Security-tool experimentation
Note on isolation: A NAT Network allows connected virtual machines to communicate with one another and access the Internet through VirtualBox. It does not provide complete isolation from external networks. For malware analysis or exercises requiring strict isolation, use a separate Internal Network without Internet access.

⚠️ Ethical use: This lab must only be used to test systems you own or have explicit permission to assess.

🏗️ Lab Architecture

The lab consists of a Linux Mint host running VirtualBox, with Kali Linux connected to a dedicated NAT Network.

text
                 Internet
                    |
              VirtualBox NAT
                    |
          NatNetwork: 10.0.0.0/24
                    |
              Kali Linux VM
              10.0.0.2/24
                    |
          Future lab target VMs


Additional target machines can be connected to the same NAT Network in future projects to practice authorized penetration testing in a controlled environment.

⚙️ Lab Configuration
🧩 Component	⚙️ Configuration
🖥️ Host OS	Linux Mint
🧠 Host RAM	8 GB
⚡ Processor	Intel Core i5-7200U
🧰 Hypervisor	VirtualBox 7.0.20
🐉 Security OS	Kali Linux 2026.2
🧠 Kali RAM	4096 MB (4 GB)
⚙️ Kali CPU	2 cores
🌐 Virtual Network	NAT Network (NatNetwork)
📡 Network Address	10.0.0.0/24
🐧 Kali IP Address	10.0.0.2/24 (static)
🚪 Default Gateway	10.0.0.1
🌍 DNS Server	8.8.8.8

Resource note: Kali is allocated 4 GB of the host's 8 GB RAM, so avoid running too many memory-intensive applications on the host and guest simultaneously.

🪜 Lab Setup Procedure
Step 1. Install 7-Zip

7-Zip or a compatible archive utility was used to extract the Kali Linux virtual-machine package downloaded as a .7z archive.

On Linux Mint, an archive utility can be installed using:

bash
sudo apt update
sudo apt install p7zip-full


Package availability can vary by Linux Mint version. If p7zip-full is unavailable, use the archive utility provided by your distribution or another compatible 7z extractor.

Step 2. Install VirtualBox

VirtualBox was installed on the Linux Mint host to create and manage virtual machines.

Official download page:

https://www.virtualbox.org/wiki/Downloads

After installation, VirtualBox was opened and prepared for importing the Kali Linux virtual machine.

Step 3. Create the NAT Network

A dedicated NAT Network was created in VirtualBox through the Network Manager.

Configuration:

text
Network Name: NatNetwork
IPv4 Prefix:  10.0.0.0/24
DHCP:         Enabled
IPv6:         Disabled


VirtualBox may suggest a different default network prefix, so the IPv4 prefix was changed to 10.0.0.0/24 and applied.

A NAT Network was selected because virtual machines connected to the same network can communicate with one another while retaining outbound Internet access through NAT.

This configuration supports future exercises involving an attacker machine and one or more target machines.

Step 4. Import and Configure Kali Linux

The Kali Linux virtual-machine package was extracted and imported into VirtualBox.

The VM was configured with the following resources:

text
Operating System: Kali Linux
RAM:              4096 MB
CPU Cores:        2
Network Adapter:  Adapter 1
Attached To:      NAT Network
Network Name:     NatNetwork


The VM network adapter was configured under:

Settings → Network → Adapter 1

After starting Kali Linux, the network connection was checked to confirm that it had joined the intended virtual network.

DHCP initially assigned the address 10.0.0.4, confirming that the virtual machine could obtain an address from the network's DHCP service.

Step 5. Configure a Static IP Address

The active NetworkManager connection name was identified using:

bash
nmcli connection show


The connection was identified as Wired connection 1, associated with interface eth0.

The following commands configured the static IPv4 address, gateway, and DNS server:

bash
sudo nmcli connection modify "Wired connection 1" \
  ipv4.method manual \
  ipv4.addresses 10.0.0.2/24 \
  ipv4.gateway 10.0.0.1 \
  ipv4.dns 8.8.8.8

sudo nmcli connection up "Wired connection 1"


The static address provides a consistent reference point for future lab exercises and documentation.

Important: Ensure that 10.0.0.2 is not already assigned to another machine and that the address does not conflict with the DHCP pool. Configure the DHCP range or reservation appropriately in VirtualBox if necessary.

Step 6. Create a Clean VM Snapshot

After completing the initial configuration and verification, a clean snapshot was created in VirtualBox.

Snapshot name:

text
Clean Kali - Network Setup


The snapshot provides a recovery point if future exercises modify the VM configuration or cause the environment to stop working.

It is good practice to create a snapshot before installing additional tools or performing experimental security exercises.

🔎 Lab Verification

(/Image1.png)

The following commands can be used to verify the network configuration and installed tools.

✅ Test	🧾 Command	🎯 Expected Result
🌐 Check IP address	ip a	eth0 shows 10.0.0.2/24
📡 Test gateway	ping -c 3 10.0.0.1	Successful replies from the gateway
🌍 Test Internet connectivity	ping -c 3 8.8.8.8	Successful replies, if ICMP is permitted
🔎 Test DNS resolution	nslookup networkwalks.com	Domain resolves using the configured DNS server
🧰 Verify Nmap	nmap --version	Installed Nmap version is displayed
📸 Verify snapshot	VirtualBox Snapshots	Clean baseline snapshot is listed

(/Image2.png)

Useful additional commands:

bash
ip route
cat /etc/resolv.conf


These commands help verify the routing table, network interface status, saved connection configuration, and DNS settings.

Verification note: Expected results are documented above. Confirm each result on the running VM before treating the corresponding test as successful.
🐞 Problems Encountered & Solutions

Documenting problems and their resolutions helps make the project reproducible.

The NAT Network was initially created using VirtualBox's suggested prefix, 10.0.2.0/24, which did not match the intended lab design.

Solution: Opened the NAT Network settings, changed the IPv4 prefix to 10.0.0.0/24, and applied the configuration.



After modifying the NetworkManager connection, ip a continued to display the previous DHCP address, 10.0.0.4.

Cause: The connection profile had been modified, but the active connection had not yet been reapplied.


Then verified the address:


The interface should show 10.0.0.2/24 once the configuration has been applied successfully.


The command using "wired connection 1" returned a message indicating that the connection profile could not be found.

Cause: The connection profile name did not exactly match the saved name.

Solution: Listed the available connection profiles:


Then used the exact connection name, including capitalization:

text
Wired connection 1



A static IP configuration can conflict with the DHCP pool or with another VM using the same address.

Solution: Verify the NAT Network's address range and DHCP configuration, ensure that 10.0.0.2 is unique, and check the active settings using:

ip a
ip route

(/Image3.png)

💡 What I Learned
1. NAT vs. NAT Network

Standard NAT provides a VM with outbound network access. A NAT Network additionally allows connected VMs to communicate over their shared virtual network, making it useful for multi-machine lab environments.

2. Virtual Machine Networking

VirtualBox network adapter modes determine how virtual machines communicate with the host, other VMs, and external networks.

3. Static IP Configuration

NetworkManager can configure a static IPv4 address, default gateway, and DNS server. Applying a modified connection is necessary for the active interface to use the new settings.

4. VM Snapshots

A clean snapshot provides a known-good recovery point before potentially disruptive experiments.

5. Resource Allocation

Allocating 4 GB of RAM and two CPU cores to Kali provides resources for common security tools while leaving some capacity for the Linux Mint host. Actual performance depends on workload and available system resources.

6. Documentation and Troubleshooting

Recording commands, expected results, configuration choices, and troubleshooting steps makes cybersecurity projects easier to reproduce and maintain.

🔐 Security & Ethical Use

This laboratory is intended for educational and authorized security testing only.

Test only systems you own or have explicit permission to assess.
Verify network isolation before running potentially dangerous experiments.
Use a separate Internal Network for exercises that must not have Internet access.
Take snapshots before making significant system changes.
Never assume that NAT alone guarantees complete isolation.
🔗 Tools & Resources
7-Zip: https://7-zip.org/download.html
VirtualBox: https://www.virtualbox.org/wiki/Downloads
Kali Linux: https://www.kali.org/get-kali/
Linux Mint: https://linuxmint.com/
Nmap: https://nmap.org/
👤 Author

Kolawole Asmot Omowumi
Cybersecurity Intern
Networkwalks

GitHub: https://github.com/omolola-cyber
LinkedIn: https://www.linkedin.com/in/kolawole-omolola-8490033a7
📌 Project Information
Internship: Cybersecurity Internship
Week: 01
Project: Cybersecurity & Pentesting Lab Setup
Organization: Networkwalks
Host OS: Linux Mint
Virtualization Platform: VirtualBox 7.0.20
Security Distribution: Kali Linux 2026.2
Repository: GitHub

⭐ This project is part of my cybersecurity learning journey at Networkwalks, documenting practical skills in virtualization, network configuration, troubleshooting, and penetration-testing lab preparation.