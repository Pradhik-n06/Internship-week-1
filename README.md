# Cybersecurity Lab Environment Setup

> Building a virtual lab for cybersecurity and ethical hacking practice

## Project Overview

This project documents the setup of a virtual cybersecurity laboratory using Oracle VirtualBox and Kali Linux on a Windows host. The goal is to create a controlled environment for learning cybersecurity fundamentals, understanding virtual machine networking, and preparing for future security-testing exercises.

The setup includes installing the required software, downloading and verifying the Kali Linux VirtualBox image, creating a virtual network, adding the virtual machine to VirtualBox, and booting to the Kali Linux desktop.

## Objectives

- Install and configure Oracle VirtualBox.
- Install 7-Zip to extract the Kali Linux virtual machine archive.
- Download and verify the Kali Linux VirtualBox image using its SHA-256 checksum.
- Add and configure Kali Linux as a virtual machine.
- Create a virtual lab network named `CyberLab`.
- Understand virtual machine network adapter settings.
- Start Kali Linux and confirm that the desktop loads successfully.
- Inspect and verify the virtual machine's network configuration.
- Capture screenshots documenting the setup process.

## Lab Architecture

The lab runs Kali Linux as a virtual machine on a Windows host using Oracle VirtualBox.

```text
Physical Computer (Windows Host)
└── Oracle VirtualBox
    └── Kali Linux VM
        └── Virtual Network Adapter
            └── CyberLab Virtual Network (10.0.0.0/24)
```

Additional virtual machines can be added to the lab for future exercises if required.

## Lab Configuration

| Component | Configuration | Status / Notes |
|---|---|---|
| Host operating system | Windows | — |
| Hypervisor | Oracle VirtualBox 7.2.22 | Installed |
| Archive utility | 7-Zip | Installed |
| Security operating system | Kali Linux 2026.3 | Desktop successfully booted |
| Kali image type | Prebuilt VirtualBox image, AMD64 | Downloaded and extracted |
| Virtual network name | `CyberLab` | Created |
| IPv4 network prefix | `10.0.0.0/24` | Configured |
| DHCP on CyberLab | Disabled | As shown in VirtualBox |
| IPv6 on CyberLab | Disabled | As configured |
| VM network attachment | NAT Network | The adapter's selected network still needs verification; the settings previously showed `NatNetwork` rather than `CyberLab`. |
| VM memory allocation | Not recorded | Verify in the VM settings. |
| Guest IP address and gateway | Not verified | Inspect from Kali Linux. |
| Internet/LAN connectivity | Not verified | Test after confirming the adapter and gateway. |

## Setup Procedure

### 1. Prepare the virtualization environment

Prepared the Windows computer for running virtual machines and obtained the software required for the lab.

### 2. Install 7-Zip

Downloaded and installed 7-Zip to extract the Kali Linux VirtualBox archive (`.7z`).

[7-Zip official website](https://www.7-zip.org/)

### 3. Install Oracle VirtualBox

Downloaded and installed Oracle VirtualBox for Windows to create and manage the Kali Linux virtual machine.

[VirtualBox downloads](https://www.virtualbox.org/wiki/Downloads)

### 4. Download Kali Linux

Downloaded the prebuilt Kali Linux 2026.3 VirtualBox AMD64 archive from the official Kali Linux website.

[Kali Linux virtual machines](https://www.kali.org/get-kali/#kali-virtual-machines)

### 5. Verify the Kali Linux download

Used Windows PowerShell to calculate the SHA-256 checksum of the downloaded archive:

```powershell
Get-FileHash "$HOME\Downloads\kali-linux-2026.3-virtualbox-amd64.7z" -Algorithm SHA256
```

Checksum obtained:

```text
84863c4fa46b2f1b29b152348583c6fb20f68f91971617c0d684e0eb163aab6d
```

The calculated checksum matched the checksum value supplied for the downloaded archive.

### 6. Extract the VirtualBox image

Used 7-Zip to extract the downloaded archive. The extracted files included the VirtualBox machine configuration (`.vbox`) and virtual disk (`.vdi`).

The virtual machine files should be kept in a permanent folder rather than a temporary Windows directory.

### 7. Add the Kali Linux virtual machine

Opened the extracted `.vbox` configuration file in VirtualBox Manager and added the Kali Linux virtual machine.

During setup, VirtualBox reported a duplicate VM UUID and later a missing virtual disk path because the VM configuration referenced a temporary extraction location. The VM files were moved to a persistent location, and the machine was made available in VirtualBox Manager.

### 8. Create the CyberLab virtual network

Opened **VirtualBox Manager → Network → NAT Networks** and created a network with these settings:

| Setting | Value |
|---|---|
| Name | `CyberLab` |
| IPv4 prefix | `10.0.0.0/24` |
| DHCP | Disabled |
| IPv6 | Disabled |

The CyberLab network appeared in the VirtualBox network list.

### 9. Configure the Kali VM network adapter

Opened **Kali VM → Settings → Network → Adapter 1**. The intended configuration is:

- **Enable Network Adapter:** Enabled
- **Attached to:** NAT Network
- **Name:** CyberLab
- **Virtual Cable Connected:** Enabled

**Verification pending:** Confirm that the Adapter 1 **Name** field is set to `CyberLab`. The VM settings screenshot previously showed `NatNetwork`, so the VM should not be assumed to be connected to CyberLab yet.

### 10. Start Kali Linux

Started the Kali Linux virtual machine from VirtualBox Manager. At the login screen, used the default credentials for the official prebuilt Kali VirtualBox image:

- **Username:** `kali`
- **Password:** `kali`

Successfully logged in and reached the Kali Linux desktop. Change the default password if the VM will be retained or used beyond a temporary lab.

### 11. Verify the network configuration

Open a terminal in Kali Linux using **Ctrl + Alt + T** and inspect the interfaces and assigned IP addresses:

```bash
ip -br addr
```

List the NetworkManager connections:

```bash
nmcli connection show
```

After confirming the correct network configuration and actual gateway, test connectivity. `10.0.0.1` is only a planned test target; confirm the gateway before relying on it:

```bash
ping -c 4 10.0.0.1
```

**Current status:** The Kali desktop has been reached. Network interface inspection, guest IP configuration, gateway confirmation, and connectivity testing are still pending. Record the command output and actual results here after verification.

## Problems Encountered and Troubleshooting

### Duplicate virtual machine UUID

**Issue:** VirtualBox reported that the VM configuration had the same UUID as an already registered virtual machine.

**Troubleshooting:**

- Checked the existing virtual machines in VirtualBox Manager.
- Removed the unwanted duplicate registration using **Remove Only**, where appropriate.
- Preserved the extracted virtual disk instead of deleting it.
- Added the VM configuration from its permanent folder.

**Result:** The Kali virtual machine became available in VirtualBox Manager.

### Virtual disk file not found

**Issue:** VirtualBox displayed `VERR_FILE_NOT_FOUND` because the VM configuration referenced a `.vdi` file in a temporary extraction directory.

**Troubleshooting:**

- Extracted the Kali archive into a permanent folder.
- Checked that the `.vdi` virtual disk file was present.
- Reconnected the VM configuration to the correct extracted files.

**Result:** The VM was able to start and reach the Kali Linux login screen.

### CyberLab not available in the adapter dropdown

**Issue:** The CyberLab network was not initially visible in the VM's NAT Network selection list.

**Troubleshooting:**

- Opened VirtualBox Manager's Network settings.
- Checked the NAT Networks list.
- Created and configured CyberLab with the required IPv4 prefix.
- Returned to the Kali VM's Adapter 1 settings.

**Result:** CyberLab appeared in the VirtualBox NAT Networks list. The final adapter selection should still be checked in the VM settings.

## What I Learned

- **Virtualization:** How Oracle VirtualBox runs an operating system inside a virtual machine.
- **Kali Linux:** How to start and access a Linux-based cybersecurity environment.
- **Virtual networking:** How NAT Network settings and virtual network adapters connect virtual machines.
- **IP addressing:** How IPv4 network prefixes define a virtual network.
- **File integrity:** How SHA-256 checksums can be used to verify downloaded files.
- **Troubleshooting:** How to investigate VM UUID conflicts and missing virtual disk files.
- **Documentation:** Why screenshots, command outputs, and clear setup instructions are important in technical projects.

## Security and Ethical Use

This laboratory is intended for educational purposes and authorized cybersecurity testing only. Use cybersecurity tools only on systems you own or have explicit permission to test. Any future scanning, penetration testing, or vulnerability assessment must be performed only on systems for which permission has been granted.

## Tools and Resources

- [Oracle VirtualBox](https://www.virtualbox.org/)
- [Kali Linux](https://www.kali.org/)
- [7-Zip](https://www.7-zip.org/)
- [VirtualBox User Manual](https://www.virtualbox.org/manual/)

## Project Information

| Field | Details |
|---|---|
| Project title | Cybersecurity Lab Environment Setup Using VirtualBox and Kali Linux |
| Host OS | Windows |
| Virtualization platform | Oracle VirtualBox |
| Guest OS | Kali Linux 2026.3 |
| Virtual network | CyberLab (`10.0.0.0/24`) |
| Repository | GitHub |

## Author

- **Name:** Pradhik Nair
- **Program:** Internship
- **Organization:** Networkwalks
- **Project:** Cybersecurity Lab Environment Setup

---

This repository documents the setup of a Kali Linux virtual cybersecurity lab and provides a foundation for future learning and practical exercises.
