# Internship-week-1
🔐 **Cybersecurity Lab Environment Setup
Building a Virtual Lab for Cybersecurity and Ethical Hacking Practice**
**📌 Project Overview**
This project documents the setup of a virtual cybersecurity laboratory
using Oracle VirtualBox and Kali Linux on a Windows host
machine.

The purpose of this lab is to create a controlled environment for
learning cybersecurity fundamentals, understanding virtual machine
networking, and preparing for future security-testing exercises.

The work includes installing the required software, downloading and
verifying the Kali Linux VirtualBox image, configuring a virtual
network, importing the virtual machine, and successfully booting into
the Kali Linux desktop.
**🎯 Objectives**
The main objectives of this project are to:
- Install and configure Oracle VirtualBox.
- Install 7-Zip to extract the Kali Linux virtual machine archive.
- Download and verify the Kali Linux VirtualBox image using its SHA256
  checksum.
- Import and configure Kali Linux as a virtual machine.
- Understand virtual machine network adapter settings.
- Create a virtual lab network named CyberLab.
- Start Kali Linux and confirm that the desktop loads successfully.
- Inspect and verify the virtual machine's network configuration.
- Capture screenshots documenting the setup process.
**🛡️ Purpose of the Lab**
This laboratory provides a controlled environment for cybersecurity
learning and authorized security testing.

Future exercises may include:
- Network reconnaissance
- Port scanning
- Vulnerability assessment
- Packet analysis
- Web security testing
- Security tool experimentation
**⚠️ Ethical Use: Use cybersecurity tools only on systems you own or
have explicit permission to test**.

**🏗️ Lab Architecture**
The lab uses Oracle VirtualBox to run Kali Linux as a virtual machine on
a Windows host.
          Physical Computer
            (Windows Host)
                  |
                  |
          Oracle VirtualBox
                  |
                  |
           Kali Linux VM
                  |
                  |
         Virtual Network Adapter
                  |
                  |
       CyberLab Virtual Network
          10.0.0.0/24
Additional virtual machines can be added to the lab in future exercises
if required.
**⚙️ Lab Configuration**
  Component                           Configuration
  Host Operating System               Windows

  Hypervisor                          Oracle VirtualBox 7.2.22

  Archive Utility                     7-Zip

  Security Operating System           Kali Linux 2026.3

  Kali Image Type                     Prebuilt VirtualBox image, AMD64

  Virtual Network Name                CyberLab

  IPv4 Network Prefix                 10.0.0.0/24

  DHCP on CyberLab                    Disabled, as shown in VirtualBox

  IPv6 on CyberLab                    Disabled, as configured

  VM Network Attachment               NAT Network (CyberLab selection
should be confirmed in VM settings)
Kali Desktop                        Successfully booted
Note: VM memory allocation, the guest IP address, gateway, and
successful internet/LAN connectivity have not yet been verified. Update
this table after checking the actual VM settings and Kali network
configuration.
**🪜 Lab Setup Procedure**
Step 1: Prepare the Virtualization Environment
Prepared the Windows computer for running virtual machines and obtained
the software required for the lab.
Step 2: Install 7-Zip
Downloaded and installed 7-Zip to extract the Kali Linux VirtualBox
archive (.7z).

Official website: https://www.7-zip.org/
Step 3: Install Oracle VirtualBox
Downloaded and installed Oracle VirtualBox for Windows to create and
manage the Kali Linux virtual machine.

Official website: https://www.virtualbox.org/wiki/Downloads
Step 4: Download Kali Linux
Downloaded the prebuilt Kali Linux 2026.3 VirtualBox AMD64 archive
from the official Kali Linux website.

Official website: https://www.kali.org/get-kali/#kali-virtual-machines
Step 5: Verify the Kali Linux Download
Used Windows PowerShell to calculate the SHA256 checksum of the
downloaded archive.

Command used:
Get-FileHash "$HOME\Downloads\kali-linux-2026.3-virtualbox-amd64.7z" -Algorithm SHA256
Checksum obtained:
84863c4fa46b2f1b29b152348583c6fb20f68f91971617c0d684e0eb163aab6d
The calculated checksum matched the checksum value supplied for the
downloaded archive.
Step 6: Extract the VirtualBox Image
Used 7-Zip to extract the downloaded archive. The extracted files
included the VirtualBox machine configuration (.vbox) and virtual disk
(.vdi).

The virtual machine files should be kept in a permanent folder rather
than a temporary Windows directory.
Step 7: Import the Kali Linux Virtual Machine
Opened the extracted .vbox configuration file in VirtualBox Manager
and added the Kali Linux virtual machine.

During setup, VirtualBox reported a duplicate VM UUID and later a
missing virtual disk path because the VM configuration referenced a
temporary extraction location. The VM files were placed in a persistent
location and the machine was made available in VirtualBox Manager.
Step 8: Create the CyberLab Virtual Network
Opened VirtualBox Manager → Network → NAT Networks and created a
network with the following settings:

  Setting       Value
  Name          CyberLab
IPv4 Prefix   10.0.0.0/24
Enable DHCP   Disabled
Enable IPv6   Disabled

The CyberLab network appeared in the VirtualBox network list.
Step 9: Configure the Kali VM Network Adapter
Opened Kali VM → Settings → Network → Adapter 1.

The intended adapter configuration is:
- Enable Network Adapter: Enabled
- Attached to: NAT Network
- Name: CyberLab
- Virtual Cable Connected: Enabled
Verification pending: Confirm that the Adapter 1 Name field is set
to CyberLab. The VM settings screenshot previously showed
NatNetwork, so do not assume that the VM was already connected to
CyberLab.
Step 10: Start Kali Linux
Started the Kali Linux virtual machine from VirtualBox Manager.

At the login screen, used the default credentials for the official
prebuilt Kali VirtualBox image:
- Username: kali
- Password: kali
Successfully logged in and reached the Kali Linux desktop.

Change the default password if the VM will be retained or used beyond a
temporary lab.
Step 11: Verify the Network Configuration
Open a terminal in Kali Linux using Ctrl + Alt + T.

Run:
ip -br addr
This displays the network interfaces and assigned IP addresses.

Then run:
nmcli connection show
This displays the NetworkManager connections.

To test connectivity after confirming the correct network configuration,
use:
ping -c 4 10.0.0.1
The gateway address above is only a planned test target. Confirm the
actual gateway before relying on it. Record the command output and
actual result here.

Status: The Kali desktop has been reached. Network interface
inspection, guest IP configuration, and connectivity testing are still
pending.
 
**🐞 Problems Encountered and Troubleshooting**
Problem 1: Duplicate Virtual Machine UUID
Issue: VirtualBox reported that the VM configuration had the same
UUID as an already registered virtual machine.

Troubleshooting:
- Checked the existing virtual machines in VirtualBox Manager.
- Removed the unwanted duplicate registration using Remove Only,
  where appropriate.
- Preserved the extracted virtual disk instead of deleting it.
- Added the VM configuration from its permanent folder.
Result: The Kali virtual machine became available in VirtualBox
Manager.
Problem 2: Virtual Disk File Not Found
Issue: VirtualBox displayed VERR_FILE_NOT_FOUND because the VM
configuration referenced a .vdi file in a temporary extraction
directory.

Troubleshooting:
- Extracted the Kali archive into a permanent folder.
- Checked that the .vdi virtual disk file was present.
- Reconnected the VM configuration to the correct extracted files.
Result: The VM was able to start and reach the Kali Linux login
screen.
Problem 3: CyberLab Not Available in the Adapter Dropdown
Issue: The CyberLab network was not initially visible in the VM's
NAT Network selection list.

Troubleshooting:
- Opened VirtualBox Manager's Network settings.
- Checked the NAT Networks list.
- Created and configured CyberLab with the required IPv4 prefix.
- Returned to the Kali VM's Adapter 1 settings.
Result: CyberLab appeared in the VirtualBox NAT Networks list. The
final adapter selection should still be checked in the VM settings.
💡 What I Learned
Through this project, I developed a better understanding of:
- Virtualization: How Oracle VirtualBox runs an operating system
  inside a virtual machine.
- Kali Linux: How to start and access a Linux-based cybersecurity
  environment.
- Virtual Networking: How NAT Network settings and virtual network
  adapters connect virtual machines.
- IP Addressing: How IPv4 network prefixes define a virtual
  network.
- File Integrity: How SHA256 checksums can be used to verify
  downloaded files.
- Troubleshooting: How to investigate VM UUID conflicts and
  missing virtual disk files.
- Documentation: Why screenshots, command outputs, and clear setup
  instructions are important in technical projects.
**🔐 Security and Ethical Use**
This laboratory is intended for educational purposes and authorized
cybersecurity testing only.

Any future scanning, penetration testing, or vulnerability assessment
must be performed only on systems for which permission has been granted.
**🔗 Tools and Resources**
- Oracle VirtualBox
- Kali Linux
- 7-Zip
- VirtualBox User Manual
👤 Author
- Name: Pradhik Nair
- Program: Internship
- Organization: Networkwalks
- Project: Cybersecurity Lab Environment Setup
📌 Project Information
  Field                               Details
  Project Title                       Cybersecurity Lab Environment Setup
Using VirtualBox and Kali Linux

  Host OS                             Windows

  Virtualization Platform             Oracle VirtualBox

  Guest OS                            Kali Linux 2026.3

  Virtual Network                     CyberLab (10.0.0.0/24)
Repository                          GitHub
This repository documents the setup of a Kali Linux virtual
cybersecurity lab and provides a foundation for future learning and
practical exercises.
