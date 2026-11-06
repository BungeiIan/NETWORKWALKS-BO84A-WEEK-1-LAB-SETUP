<div align="center">

# 🔐 Cybersecurity Lab Environment Setup

**Building an isolated virtual lab for penetration testing and ethical hacking practice**
</div>

<p align="center">
  <img src="https://img.shields.io/badge/Skill-Cybersecurity-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Hypervisor-VirtualBox-0070C0?style=flat-square&labelColor=000000" />
  <img src="https://img.shields.io/badge/Kali%20Linux-v2026.3-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Skill-Linux-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Network-10.0.0.0%2F24-238F89?style=flat-square&labelColor=000000" />
  <img src="https://img.shields.io/badge/Penetration%20Testing-C00000?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Skill-Virtualization-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/GitHub-404040?style=flat-square&labelColor=0070C0&logo=github&logoColor=white" />
  <img src="https://img.shields.io/badge/Kali%20Linux-404040?style=flat-square&labelColor=C00000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Ethical%20Hacking-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
</p>

---

## 📌 Project Overview

This project focuses on setting up a **virtual cybersecurity and penetration-testing laboratory** using VirtualBox and Kali Linux.

The purpose of the lab is to create a controlled environment where cybersecurity tools, network scanning, reconnaissance, vulnerability assessment, and other security-testing activities can be performed safely and repeatedly.

The lab is configured on a private virtual network so that additional machines can be added later and used as targets for authorized security testing.

---

## 🎯 Objectives

The main objectives of this project are to:

- Install and configure VirtualBox.
- Install/import Kali Linux as a virtual machine.
- Create a private **NAT Network** for the cybersecurity lab.
- Configure network connectivity for Kali Linux.
- Assign a consistent static IP address to the Kali VM.
- Verify network connectivity and DNS resolution.
- Take a clean VM snapshot for recovery.
- Document the complete setup process.
- Prepare the environment for future cybersecurity projects.

---

## 🛡️ Purpose of the Lab

The lab provides an isolated and controlled environment for cybersecurity learning and authorized security testing.

It can be used for activities such as:

- Network reconnaissance
- Port scanning
- Vulnerability assessment
- Packet analysis
- Web security testing
- Exploitation practice
- Security-tool experimentation

⚠️ **Important:** This laboratory must only be used for systems that you own or have explicit permission to test. Do not use the lab or its tools to attack unauthorized systems.

---

## 🏗️ Lab Architecture

```text
                 ┌──────────────────────────────┐
                 │  Windows Host (VirtualBox)   │
                 │         ↕ Internet           │
                 └──────────────┬───────────────┘
                                │  NAT gateway 10.0.0.1
               ┌────────────────┴────────────────┐
               │   NatNetwork1  (10.0.0.0/24)    │
               └───────┬─────────────────┬───────┘
                       │                 │
             ┌─────────┴───────┐  ┌──────┴─────────────┐
             │  Kali Linux     │  │  Future target VMs  │
             │  10.0.0.2       │  │  10.0.0.3 – 10.0.0.99│
             └─────────────────┘  └─────────────────────┘
```

Additional target machines can be added to the same virtual network in future projects.

![Kali Linux running in VirtualBox](docs/images/kali_screenshot.png)

---

## ⚙️ Lab Configuration

| 🧩 Component       | ⚙️ Configuration                     |
| ------------------ | ------------------------------------ |
| 🖥️ Host OS         | Windows 11                           |
| 🧠 Host RAM        | 8gb                         |
| ⚡ Processor       | Intel(R) Core(TM) i5-6300U CPU @ 2.40GHz, 2496 Mhz, 2 Core(s), 4 Logical Processor(s)
                         |
| 🧰 Hypervisor      | Oracle VirtualBox                    |
| 🐉 Security OS     | Kali Linux 2026.3 (amd64)            |
| 🧠 Kali RAM        | 4gb                          |
| 🌐 Virtual Network | NAT Network (`NatNetwork1`)          |
| 📡 Network Address | 10.0.0.0/24                          |
| 🐧 Kali IP Address | 10.0.0.2/24 (static)                 |
| 🚪 Default Gateway | 10.0.0.1                             |
| 🌍 DNS Server      | 10.0.0.1                             |
| 🔮 Future VM Range | 10.0.0.3 – 10.0.0.99                 |

---

# 🪜 Lab Setup Procedure

## Step 1. Install 7-Zip

7-Zip is used to extract the Kali Linux virtual-machine package, which is distributed as a `.7z` archive.

**Tool:** 7-Zip

---

## Step 2. Install VirtualBox

Oracle VirtualBox was installed as the hypervisor.

---

## Step 3. Create the NAT Network

A dedicated NAT Network was created in VirtualBox (**File → Tools → Network Manager → NAT Networks → Create**).

Configuration:

```text
Network Name: NatNetwork1
IPv4 Prefix:  10.0.0.0/24
DHCP:         Enabled
IPv6:         Disabled
```

![NAT Network settings in VirtualBox](docs/images/NATnetwork_screenshot.png)

A **NAT Network** was selected because multiple virtual machines connected to the same NAT Network can communicate with one another while also having outbound network connectivity.

This will allow future attacker and target VMs to communicate within the lab.

> ⚠️ **Note:** Avoid keeping two NAT Networks with the same prefix (for example, `NatNetwork` and `NatNetwork1` both on `10.0.0.0/24`). Delete the unused one or give it a different prefix to avoid conflicts.

---

## Step 4. Import Kali Linux

The Kali Linux virtual machine was downloaded from the official Kali Linux website and imported into VirtualBox as `kali-linux-2026.3-virtualbox-amd64`.

![VirtualBox Manager showing the Kali VM](docs/images/virtual_box_screenshot.png)

With the VM powered off, the network adapter was configured as follows (**Settings → Network**):

```text
Adapter 1
Attached to: NAT Network
Name:        NatNetwork1
```

---

## Step 5. Configure the Kali Linux Network

The Kali Linux network was configured with a consistent static IPv4 address through the **Wired connection 1 → IPv4 Settings** tab.

```text
Method:      Manual
IP Address:  10.0.0.2
Netmask:     24 (255.255.255.0)
Gateway:     10.0.0.1
DNS:         10.0.0.1
```

A consistent IP address makes it easier to document the lab and reference the Kali machine in future exercises.

![Kali IPv4 manual settings](docs/images/kali_network_screenshot.png)

The same configuration can be applied from the terminal:

```bash
nmcli connection modify "Wired connection 1" \
  ipv4.method manual \
  ipv4.addresses 10.0.0.2/24 \
  ipv4.gateway 10.0.0.1 \
  ipv4.dns 10.0.0.1

nmcli connection down "Wired connection 1" && nmcli connection up "Wired connection 1"
```

---

## Step 6. Create a Clean VM Snapshot

After completing the initial configuration, take a VirtualBox snapshot (**Machine Tools → Snapshots → Take**).

Suggested snapshot name:

```text
Clean Kali - Network Setup
```

The snapshot represents the clean baseline of the laboratory.

If a future exercise changes or damages the VM configuration, the machine can be restored to this baseline.

---

# 🔎 Lab Verification

| ✅ Test                        | 🧾 Command                      | 🎯 Expected Result              |
| ----------------------------- | ------------------------------- | ------------------------------- |
| 🌐 Check IP address           | `ip a`                          | Correct Kali IP displayed       |
| 🛣️ Check default route        | `ip route`                      | Default via 10.0.0.1            |
| 📡 Test gateway               | `ping -c 4 10.0.0.1`            | Successful replies              |
| 🌍 Test Internet connectivity | `ping -c 4 8.8.8.8`             | Successful replies              |
| 🔎 Test DNS resolution        | `nslookup google.com`           | Domain resolves                 |
| 🧰 Verify Nmap                | `nmap --version`                | Nmap version displayed          |
| 🔄 Verify snapshot            | Restore snapshot and run `ip a` | Baseline configuration restored |

### Example Results

```text
IP Address:
10.0.0.2/24

Gateway:
10.0.0.1

DNS:
10.0.0.1
```

---

# 🐞 Common Problems & Solutions

Documenting problems is an important part of the project.

## Problem 1. Internet Connectivity After Static IP Configuration

After manually configuring the IPv4 settings, Internet connectivity may fail depending on the Kali/NetworkManager configuration.

One possible workaround:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
```

Then restart the connection (or reboot) and test connectivity again.

> **Important:** Connection names may differ between systems. Run `nmcli connection show` first to identify your actual connection name.

---

## Problem 2. VirtualBox VT-x / Virtualization Error

If the VM fails to start, hardware virtualization may be disabled in the system firmware/BIOS.

Fix:

1. Restart the computer.
2. Enter BIOS/UEFI settings.
3. Enable Intel VT-x / AMD-V (hardware virtualization).
4. Save the configuration and restart.
5. Start the Kali VM again.

---

## Problem 3. Two NAT Networks With the Same Prefix

Having two NAT Networks on `10.0.0.0/24` can cause routing and addressing conflicts.

Fix: remove the unused NAT Network, or change its IPv4 prefix (e.g. `10.0.1.0/24`) in **Network Manager → NAT Networks**.

---

# 💡 What I Learned

Through this project, I learned how to create and configure a virtual environment for cybersecurity practice.

### 1. NAT vs NAT Network

A standard NAT adapter isolates each VM. A NAT Network lets multiple VMs on the same virtual network communicate with one another while still providing network address translation for external connectivity. This makes it ideal for a multi-machine cybersecurity lab.

### 2. Virtual Machine Networking

I learned how VirtualBox virtual network adapters connect virtual machines to different types of networks and how network configuration affects communication between machines.

### 3. Static IP Configuration

I learned how to configure and verify IPv4 addressing, subnet masks, gateways, and DNS settings in Kali Linux.

### 4. VM Snapshots

A clean snapshot should be created **before performing risky or experimental activities**. It provides a known-good recovery point for future exercises.

### 5. Documentation

Documenting commands, configuration, screenshots, problems, and solutions is an important part of a professional cybersecurity project.

---

# 🔐 Security & Ethical Use

This laboratory is intended strictly for educational purposes and authorized security testing only.

---

# 🔗 Tools & Resources

- **7-Zip:** [https://7-zip.org/download.html](https://7-zip.org/download.html)
- **VirtualBox:** [https://virtualbox.org/wiki/Downloads](https://virtualbox.org/wiki/Downloads)
- **Kali Linux:** [https://kali.org/get-kali](https://kali.org/get-kali)

---

# 👤 Author

**Ian Bungei Imbayi**\
Software Engineering Student, Kisii University

GitHub: [https://github.com/BungeiIan](https://github.com/BungeiIan)\
LinkedIn: [https://www.linkedin.com/in/BungeiIan/](https://www.linkedin.com/in/bungeiIan/)

---

## 📌 Project Information

**Project:** Cybersecurity & Pentesting Lab Setup | **Repository:** GitHub
