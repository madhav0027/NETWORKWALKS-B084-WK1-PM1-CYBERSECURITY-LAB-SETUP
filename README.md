🛡️ Cybersecurity & Pentesting Lab Setup

<p align="center">
<img src="https://img.shields.io/badge/Networkwalks-Cybersecurity-blue?style=for-the-badge" alt="Networkwalks"/>
<img src="https://img.shields.io/badge/Project-01-purple?style=for-the-badge" alt="Project 01"/>
<img src="https://img.shields.io/badge/VirtualBox-7.0.20-183A61?style=for-the-badge&logo=virtualbox&logoColor=white" alt="VirtualBox"/>
<img src="https://img.shields.io/badge/Kali%20Linux-2026.2-557C94?style=for-the-badge&logo=kalilinux&logoColor=white" alt="Kali Linux"/>
</p>

<p align="center">
<b>Building a Virtual Cybersecurity Lab for Ethical Hacking & Network Security</b>
<br/>
<i>Cybersecurity Internship | Week 01 | Networkwalks</i>
</p>

⚡ Project Overview

Welcome to my first cybersecurity internship project at Networkwalks!

This project documents the setup of a virtual penetration-testing lab using VirtualBox and Kali Linux, hosted on Linux Mint.

The goal is to build a controlled environment where I can practice network reconnaissance, port scanning, vulnerability assessment, and other cybersecurity techniques without directly modifying my primary operating system.

By using a dedicated virtual network, the lab can also be expanded with additional virtual machines for authorized security testing.

🎯 Project Objectives
🖥️ Install and configure VirtualBox on Linux Mint.
🐉 Import and configure Kali Linux.
🌐 Create a dedicated VirtualBox NAT Network.
📡 Configure a static IP address for Kali Linux.
🔎 Verify connectivity, routing, DNS, and Nmap.
📸 Create a clean VM snapshot for recovery.
📝 Document configuration issues and troubleshooting solutions.
🏗️ Lab Architecture

<p align="center">
<img src="https://img.shields.io/badge/HOST-Linux%20Mint-87CF3E?style=flat-square&logo=linuxmint&logoColor=white" alt="Linux Mint Host"/>
<img src="https://img.shields.io/badge/HYPERVISOR-VirtualBox-183A61?style=flat-square&logo=virtualbox&logoColor=white" alt="VirtualBox"/>
<img src="https://img.shields.io/badge/GUEST-Kali%20Linux-557C94?style=flat-square&logo=kalilinux&logoColor=white" alt="Kali Linux"/>
</p>

text
                    ┌──────────────────────┐
                    │       INTERNET       │
                    └──────────┬───────────┘
                               │
                    ┌──────────▼───────────┐
                    │   VirtualBox NAT     │
                    │   Outbound Access    │
                    └──────────┬───────────┘
                               │
              ┌────────────────────────────────┐
              │   NatNetwork: 10.0.0.0/24      │
              │                                │
              │   ┌────────────────────────┐   │
              │   │     Kali Linux VM      │   │
              │   │     10.0.0.2/24         │   │
              │   │     2 CPU / 4 GB RAM   │   │
              │   └────────────────────────┘   │
              │                                │
              │   Future Target VMs             │
              └────────────────────────────────┘

Network design: A NAT Network allows connected VMs to communicate with one another and access external networks through NAT. It does not guarantee complete isolation from the Internet or host environment. Use a dedicated Internal Network for experiments that require no external connectivity.
⚙️ Lab Specifications

<table>
<tr>
<th>Component</th>
<th>Configuration</th>
</tr>
<tr><td>🖥️ Host OS</td><td>Linux Mint</td></tr>
<tr><td>🧠 Host Memory</td><td>8 GB RAM</td></tr>
<tr><td>⚡ Processor</td><td>Intel Core i5-7200U</td></tr>
<tr><td>🧰 Hypervisor</td><td>VirtualBox 7.0.20</td></tr>
<tr><td>🐉 Guest OS</td><td>Kali Linux 2026.2</td></tr>
<tr><td>🧠 Guest Memory</td><td>4096 MB (4 GB)</td></tr>
<tr><td>⚙️ Guest CPU</td><td>2 cores</td></tr>
<tr><td>🌐 Network Mode</td><td>NAT Network</td></tr>
<tr><td>📛 Network Name</td><td><code>NatNetwork</code></td></tr>
<tr><td>📡 Subnet</td><td><code>10.0.0.0/24</code></td></tr>
<tr><td>🐧 Kali IPv4</td><td><code>10.0.0.2/24</code></td></tr>
<tr><td>🚪 Gateway</td><td><code>10.0.0.1</code></td></tr>
<tr><td>🌍 DNS</td><td><code>8.8.8.8</code></td></tr>
</table>

💡 Resource planning: Kali uses 4 GB of the host's 8 GB RAM. Keep sufficient memory available for Linux Mint, and avoid running multiple resource-intensive applications simultaneously.
🪜 Installation & Configuration
01 — 📦 Install an Archive Utility

The Kali virtual-machine package was downloaded as a .7z archive and extracted using a compatible archive utility.

On Linux Mint, try:

bash
sudo apt update
sudo apt install p7zip-full


If the package is unavailable for your Linux Mint version, use another compatible 7z extraction utility.

02 — 🧰 Install VirtualBox

VirtualBox provides the virtualization environment for running Kali Linux without replacing the host operating system.

🔗 Download: VirtualBox Official Downloads

After installation, open VirtualBox and prepare to import the Kali virtual-machine package.

03 — 🌐 Create the NAT Network

Open VirtualBox's Network Manager and create a dedicated NAT Network.

Configuration used:

ini
Network Name = NatNetwork
IPv4 Prefix  = 10.0.0.0/24
DHCP         = Enabled
IPv6         = Disabled


The network prefix was changed from VirtualBox's initial default to match the lab's addressing plan.

This network allows future attacker and target VMs to share a virtual subnet while retaining outbound Internet access.

04 — 🐉 Import and Configure Kali Linux

Import the extracted Kali Linux VM into VirtualBox.

Configure the VM with these resources:

ini
Operating System = Kali Linux
RAM              = 4096 MB
CPU Cores        = 2
Network Adapter  = Adapter 1
Attached To      = NAT Network
Network Name     = NatNetwork


Navigate to:

Settings → Network → Adapter 1

Select NAT Network and choose NatNetwork.

After booting Kali, verify that the network interface is connected. DHCP initially assigned 10.0.0.4, confirming that the VM could obtain an address.

05 — 📡 Configure a Static IP

First, identify the active NetworkManager connection:

bash
nmcli connection show


The connection used in this setup was Wired connection 1, associated with eth0.

Apply the static configuration:

bash
sudo nmcli connection modify "Wired connection 1" \
  ipv4.method manual \
  ipv4.addresses 10.0.0.2/24 \
  ipv4.gateway 10.0.0.1 \
  ipv4.dns 8.8.8.8


Reactivate the connection:

bash
sudo nmcli connection up "Wired connection 1"


Verify the result:

bash
ip a show eth0
ip route


Expected address:

text
10.0.0.2/24

⚠️ IP address safety: Ensure 10.0.0.2 is not assigned to another VM and does not conflict with the DHCP pool. Configure the DHCP range or reservation appropriately.
06 — 📸 Create a Clean Snapshot

After completing the configuration and verification, create a snapshot in VirtualBox.

Snapshot name:

text
Clean Kali - Network Setup


This snapshot acts as a recovery point before future experiments, tool installations, or configuration changes.

🔎 Lab Verification

The following checks help confirm that the environment is configured correctly.

Test	Command	Expected result
🌐 IP address	ip a	eth0 shows 10.0.0.2/24
📡 Gateway	ping -c 3 10.0.0.1	Successful replies
🌍 Internet	ping -c 3 8.8.8.8	Successful replies, if ICMP is permitted
🔎 DNS	nslookup networkwalks.com	Domain resolves successfully
🧰 Nmap	nmap --version	Installed version is displayed
📸 Snapshot	VirtualBox Snapshots	Baseline snapshot is present
🖼️ Verification Screenshots

<details>
<summary><b>📷 Image 1 — Network Configuration</b></summary>

Add Image1.png to your repository and embed it here:

markdown
![Network Configuration](Image1.png)


</details>

<details>
<summary><b>📷 Image 2 — Connectivity & Tool Verification</b></summary>

Add Image2.png to your repository and embed it here:

markdown
![Connectivity and Tool Verification](Image2.png)


</details>

<details>
<summary><b>📷 Image 3 — Troubleshooting / Configuration</b></summary>

Add Image3.png to your repository and embed it here:

markdown
![Troubleshooting and Configuration](Image3.png)


</details>

Additional Diagnostic Commands
bash
ip route
nmcli device status
nmcli connection show
nmcli device show eth0
cat /etc/resolv.conf


These commands help inspect routing, interface status, connection profiles, and DNS configuration.

✅ Verification note: The table lists expected results. Confirm the actual outputs on your VM before reporting a test as successful.
🐞 Troubleshooting & Solutions
🔴 Issue 1 — Incorrect Network Prefix

Problem: The NAT Network was initially created with 10.0.2.0/24, which did not match the planned subnet.

Solution: Open the NAT Network settings, change the IPv4 prefix to 10.0.0.0/24, and apply the configuration.

🟠 Issue 2 — Static IP Did Not Apply

Problem: The interface continued showing the previous DHCP address, 10.0.0.4.

Cause: The saved NetworkManager connection had not been reapplied.

Solution:

bash
sudo nmcli connection up "Wired connection 1"
ip a show eth0


Confirm that the interface now displays 10.0.0.2/24.

🟡 Issue 3 — Incorrect Connection Name

Problem: Using "wired connection 1" returned a connection-profile error.

Cause: The connection name did not match the saved profile exactly.

Solution:

bash
nmcli connection show


Use the exact connection name shown in the output:

text
Wired connection 1

🔵 Issue 4 — Possible IP Address Conflict

Problem: A static address can conflict with the DHCP pool or another VM.

Solution: Check the network configuration, DHCP range, and addresses used by other machines.

bash
ip a
ip route
nmcli device show eth0


Reserve or exclude the static address from the DHCP pool where appropriate.

💡 Key Learnings

<table>
<tr>
<td>🌐</td>
<td><b>NAT vs. NAT Network</b><br/>Learned how NAT Network supports communication between connected VMs while providing outbound connectivity.</td>
</tr>
<tr>
<td>🖥️</td>
<td><b>Virtualization</b><br/>Learned how virtual network adapters connect guest operating systems to different network environments.</td>
</tr>
<tr>
<td>📡</td>
<td><b>Static IP Configuration</b><br/>Practiced configuring IPv4, gateway, and DNS using NetworkManager.</td>
</tr>
<tr>
<td>📸</td>
<td><b>Snapshots & Recovery</b><br/>Learned the importance of maintaining a clean baseline before experimental work.</td>
</tr>
<tr>
<td>⚙️</td>
<td><b>Resource Management</b><br/>Configured Kali with 4 GB RAM and two CPU cores on an 8 GB Linux Mint host.</td>
</tr>
<tr>
<td>📝</td>
<td><b>Technical Documentation</b><br/>Practiced documenting setup steps, expected results, configuration problems, and fixes.</td>
</tr>
</table>

🔐 Security & Ethical Use

This laboratory is designed for educational purposes and authorized security testing.

✅ Test only systems you own or have explicit permission to assess.
🔒 Verify network isolation before conducting sensitive experiments.
🧪 Use a separate Internal Network for exercises requiring no Internet access.
📸 Create snapshots before making potentially disruptive changes.
⚠️ Remember that NAT alone does not guarantee complete isolation.
🔗 Tools & Resources
Tool	Purpose	Official resource
📦 7-Zip	Archive extraction	Download
🧰 VirtualBox	Virtualization	Download
🐉 Kali Linux	Security testing OS	Get Kali
🖥️ Linux Mint	Host operating system	Official website
🔎 Nmap	Network discovery and scanning	Official website
👨‍💻 Author

Kolawole Asmot Omowumi

🎓 Cybersecurity Intern
🏢 Networkwalks

<p>
<a href="https://github.com/omolola-cyber">
<img src="https://img.shields.io/badge/GitHub-Profile-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub Profile"/>
</a>
<a href="https://www.linkedin.com/in/kolawole-omolola-8490033a7">
<img src="https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn Profile"/>
</a>
</p>

📌 Project Information
Field	Details
Internship	Cybersecurity Internship
Organization	Networkwalks
Week	01
Project	Cybersecurity & Pentesting Lab Setup
Host OS	Linux Mint
Hypervisor	VirtualBox 7.0.20
Guest OS	Kali Linux 2026.2
Repository	GitHub

<p align="center">
<b>🚀 Learning by Building. Securing by Testing. Growing Through Practice.</b>
<br/><br/>
⭐ <i>This project marks the beginning of my hands-on cybersecurity journey at Networkwalks.</i>
</p>
