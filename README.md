<div align="center">

🔐 Cybersecurity Lab Environment Setup

Building an isolated virtual lab for penetration testing and ethical hacking practice

Networkwalks | Cybersecurity Internship | Project 01

</div>

<p align="center">
<img src="https://img.shields.io/badge/Skill-Cybersecurity-404040?style=flat-square&labelColor=C00000" alt="Cybersecurity"/>
<img src="https://img.shields.io/badge/VirtualBox-v7.0.20-0070C0?style=flat-square&labelColor=000000&logo=virtualbox&logoColor=white" alt="VirtualBox"/>
<img src="https://img.shields.io/badge/Kali%20Linux-v2026.2-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" alt="Kali Linux"/>
<img src="https://img.shields.io/badge/Host-Linux%20Mint-87CF3E?style=flat-square&labelColor=000000&logo=linuxmint&logoColor=white" alt="Linux Mint"/>
<img src="https://img.shields.io/badge/Network-10.0.0.0%2F24-238F89?style=flat-square&labelColor=000000" alt="Network"/>
<img src="https://img.shields.io/badge/Penetration%20Testing-C00000?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" alt="Penetration Testing"/>
<img src="https://img.shields.io/badge/Skill-Virtualization-404040?style=flat-square&labelColor=C00000" alt="Virtualization"/>
<img src="https://img.shields.io/badge/Ethical%20Hacking-E87500?style=flat-square&labelColor=000000" alt="Ethical Hacking"/>
<img src="https://img.shields.io/badge/Networkwalks-404040?style=flat-square&labelColor=C00000" alt="Networkwalks"/>
<img src="https://img.shields.io/badge/GitHub-404040?style=flat-square&labelColor=0070C0&logo=github&logoColor=white" alt="GitHub"/>
</p>

📌 Project Overview

This project focuses on setting up a virtual cybersecurity and penetration-testing laboratory using VirtualBox and Kali Linux, hosted on Linux Mint.

The purpose of this lab is to establish a controlled environment for practicing cybersecurity tools, network scanning, reconnaissance, vulnerability assessment, and other security-testing activities.

A dedicated VirtualBox NAT Network is configured to allow multiple virtual machines to communicate with each other while providing outbound network connectivity.

This environment serves as the foundation for future hands-on cybersecurity projects and authorized penetration-testing exercises.

🎯 Objectives

The main objectives of this project are to:

Install and configure VirtualBox on Linux Mint.
Import and configure Kali Linux as a virtual machine.
Create a dedicated NAT Network for the cybersecurity lab.
Configure network connectivity and a static IP address.
Verify gateway connectivity, Internet access, and DNS resolution.
Verify that Nmap is installed and working.
Create a clean virtual-machine snapshot for recovery.
Document the setup procedure, screenshots, problems, and solutions.
Prepare the environment for future cybersecurity projects.
🛡️ Purpose of the Lab

This laboratory provides a controlled virtual environment for learning and practicing authorized cybersecurity techniques.

It can be used for activities such as:

Network reconnaissance
Port scanning
Vulnerability assessment
Packet analysis
Web application security testing
Security-tool experimentation
Multi-VM penetration-testing exercises

⚠️ Ethical Use: Only test systems you own or have explicit permission to assess.

🔒 Network Isolation: A NAT Network allows connected virtual machines to communicate with each other and access external networks. For experiments requiring strict isolation, use a separate Internal Network without Internet access.

🏗️ Lab Architecture

<p align="center">
<img src="Image1.png" alt="Cybersecurity lab architecture" width="850"/>
</p>


---

## ⚙️ Lab Configuration

| 🧩 Component       | ⚙️ Configuration   |
| ------------------ | ------------------  |
| 🖥️ Host OS         | Linux Mint         |
| 🧠 Host RAM        | 8 GB               |
| ⚡ Processor       | Intel Core i5      |
| 🧰 Hypervisor      | VirtualBox 7.2  |
| 🐉 Security OS     | Kali Linux 2026.2  |
| 🧠 Kali RAM        | 4096 MB            |
| 🌐 Virtual Network | NAT Network        |
| 📡 Network Address | 10.0.0.0/24        |
| 🐧 Kali IP Address | 10.0.0.2/24        |
| 🚪 Default Gateway | 10.0.0.1           |
| 🌍 DNS Server      | 8.8.8.8            |
| 🔮 Future VM Range | 10.0.0.3–10.0.0.99 |

---

🪜 Lab Setup Procedure
Step 1. Install 7-Zip

A compatible archive utility was used to extract the Kali Linux virtual-machine package, which was downloaded as a .7z archive.

On Linux Mint, update the package lists and install an available archive utility:

bash
sudo apt update
sudo apt install p7zip-full


If the package is unavailable in your Linux Mint version, use another compatible 7-Zip extraction utility.

Purpose: Extract the Kali Linux virtual-machine files before importing them into VirtualBox.

Step 2. Install VirtualBox

VirtualBox was installed on the Linux Mint host to create and manage virtual machines.

Tool: VirtualBox 7.0.20

Official download: VirtualBox Downloads

After installation, VirtualBox was opened to import and configure the Kali Linux virtual machine.

Step 3. Create the NAT Network

A dedicated NAT Network was created through VirtualBox Network Manager.

Network Configuration
text
Network Name: NatNetwork
IPv4 Prefix:  10.0.0.0/24
DHCP:         Enabled
IPv6:         Disabled


<p align="center">
<img src="Image2.png" alt="VirtualBox NAT Network configuration" width="850"/>
</p>

The IPv4 prefix was configured as 10.0.0.0/24 to match the planned lab addressing scheme.

A NAT Network was selected because multiple virtual machines connected to the same network can communicate with one another while retaining outbound network connectivity.

This configuration prepares the lab for future attacker and target virtual machines.

Step 4. Import Kali Linux

Kali Linux was downloaded from the official Kali Linux website and imported into VirtualBox.

Official download: Get Kali Linux

Virtual Machine Configuration
text
Operating System: Kali Linux
RAM:              4096 MB
CPU Cores:        2
Network Adapter:  Adapter 1
Attached To:      NAT Network
Network Name:     NatNetwork


The network adapter was configured under:

Settings → Network → Adapter 1

After starting Kali Linux, DHCP initially assigned the address 10.0.0.4, confirming that the virtual machine could obtain an IP address from the virtual network.

A shared folder can also be configured if files need to be transferred between Linux Mint and Kali Linux.

Step 5. Configure the Kali Linux Network

The active NetworkManager connection was identified using:

bash
nmcli connection show


The connection used in this setup was Wired connection 1, associated with the eth0 interface.

Configure a Static IPv4 Address

The following commands were used to configure the static network settings:

bash
sudo nmcli connection modify "Wired connection 1" \
  ipv4.method manual \
  ipv4.addresses 10.0.0.2/24 \
  ipv4.gateway 10.0.0.1 \
  ipv4.dns 8.8.8.8

sudo nmcli connection up "Wired connection 1"


<p align="center">
<img src="Image3.png" alt="Kali Linux virtual machine configuration" width="850"/>
</p>


Network Settings
⚙️ Setting	📌 Value
IP Address	10.0.0.2
Subnet Mask	255.255.255.0
Default Gateway	10.0.0.1
DNS Server	8.8.8.8
Verify the Configuration
bash
ip a show eth0
ip route
nmcli device show eth0


A static address makes it easier to identify and reference the Kali Linux machine during future lab exercises.

⚠️ Important: Ensure that 10.0.0.2 is not already in use and does not conflict with the DHCP pool. Configure the DHCP range or reservation appropriately.

Step 6. Create a Clean VM Snapshot

After completing the initial configuration and verification, a VirtualBox snapshot was created.

Snapshot Name
text
Clean Kali - Network Setup


The snapshot represents the clean baseline of the laboratory.

If a future exercise breaks the network configuration or changes the virtual machine unexpectedly, the snapshot provides a recovery point.

🔎 Lab Verification

The following tests help verify the network configuration and installed security tools.

---

# 🔎 Lab Verification

| ✅ Test                        | 🧾 Command                      | 🎯 Expected Result              |
| ----------------------------- | ------------------------------- | ------------------------------- |
| 🌐 Check IP address           | `ip a`                          | Correct Kali IP displayed       |
| 📡 Test gateway               | `ping 10.0.0.1`                 | Successful replies              |
| 🌍 Test Internet connectivity | `ping 8.8.8.8`                  | Successful replies              |
| 🔎 Test DNS resolution        | `nslookup networkwalks.com`     | Domain resolves                 |
| 🧰 Verify Nmap                | `nmap --version`                | Nmap version displayed          |
| 🔄 Verify snapshot            | Restore snapshot and run `ip a` | Baseline configuration restored |

### Example Results

```text
IP Address:
10.0.0.2/24

Gateway:
10.0.0.1

DNS:
8.8.8.8
```

---


Useful Diagnostic Commands
bash
ip route
nmcli device status
nmcli connection show
nmcli device show eth0
cat /etc/resolv.conf


These commands help identify network interfaces, active connections, routing information, and DNS configuration.

💡 What I Learned

Through this project, I developed practical knowledge of virtualization, network configuration, troubleshooting, and cybersecurity lab management.

1. NAT vs. NAT Network

A standard NAT configuration provides outbound connectivity to a virtual machine. A NAT Network additionally allows connected virtual machines to communicate over their shared virtual network.

This makes NAT Networks useful for multi-machine cybersecurity laboratories.

2. Virtual Machine Networking

I learned how VirtualBox network adapters connect virtual machines to different network environments and how networking modes affect communication between systems.

3. Static IP Configuration

I learned how to configure IPv4 addresses, subnet masks, gateways, and DNS servers using NetworkManager in Kali Linux.

4. Virtual Machine Snapshots

I learned that a clean snapshot should be created before potentially disruptive or experimental activities to preserve a known-good recovery point.

5. Resource Allocation

I configured Kali Linux with 4 GB RAM and two CPU cores on a Linux Mint host with 8 GB RAM, balancing guest performance with host resource availability.

6. Documentation and Troubleshooting

I learned the importance of recording commands, configuration settings, screenshots, problems, and solutions as part of a professional cybersecurity workflow.

🔐 Security & Ethical Use

This laboratory is intended for educational purposes and authorized security testing only.

✅ Test only systems you own or have explicit permission to assess.
🔒 Verify network isolation before conducting sensitive experiments.
🧪 Use an Internal Network for exercises that must not have Internet access.
📸 Create snapshots before making potentially disruptive changes.
⚠️ Never assume NAT alone guarantees complete isolation.
🛡️ Keep the host operating system and virtualization software updated.
🔗 Tools & Resources
🧰 Tool	🎯 Purpose	🔗 Official Resource
📦 7-Zip	Archive extraction	7-Zip Downloads
🧰 VirtualBox	Virtualization	VirtualBox Downloads
🐉 Kali Linux	Security-testing operating system	Get Kali Linux
🖥️ Linux Mint	Host operating system	Linux Mint
🔎 Nmap	Network discovery and scanning	Nmap Official Website
👤 Author

<div align="center">

Kolawole Asmot Omowumi

Cybersecurity Intern | Networkwalks

<a href="https://github.com/madhav0027">
<img src="https://img.shields.io/badge/GitHub-Profile-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub Profile"/>
</a>
<a href="https://www.linkedin.com/in/madhav-pathak-498076384">
<img src="https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn Profile"/>
</a>

</div>

📌 Project Information
📋 Field	📝 Details
🎓 Internship	Cybersecurity Internship
🏢 Organization	Networkwalks
📅 Week	01
🔐 Project	Cybersecurity & Pentesting Lab Setup
🖥️ Host OS	Linux Mint
🧰 Hypervisor	VirtualBox 7.0.20
🐉 Guest OS	Kali Linux 2026.2
🌐 Network	NatNetwork — 10.0.0.0/24
📁 Repository	GitHub Profile

<div align="center">

🚀 Learn by Building. Secure by Testing. Grow Through Practice.

Project 01 — Cybersecurity Internship at Networkwalks

⭐ This project marks the beginning of my hands-on cybersecurity journey, developing practical skills in virtualization, networking, troubleshooting, and ethical penetration testing.

</div>
