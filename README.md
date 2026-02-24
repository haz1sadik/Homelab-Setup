# My Personal Cybersecurity Homelab Project Setup

---

## Overview

In this project, my main goal is to build and deploy my cybersecurity research lab environment. This project architecture will use **Proxmox VE** as a Type-1 Hypervisor to manage all the virtual machines that will be used in the future project and **OPNSense** as my virtual router and firewall.

**Hardware:**
- **Host Machine:**
  - HP EliteBook 830 G6 Laptop
  - 24GB RAM
  - 512GB NVMe SSD
- **Network Interface 1:** Integrated Wi-Fi (Used for Upstream Internet)
- **Network Interface 2:** USB-to-Ethernet (My RJ45 port is not working)
- **External AP:** Mercusys MW300D (So i can access Proxmox Console through Wi-Fi)
- **Management PC:** Mac Mini & Surface Pro 7
- **Future Add-On:** Managed Switch

---

## Table of Contents

1. [Network Architecture]()

2. [Security Decisions]()

3. [Proxmox Installation & Wi-Fi Setup](sections/Proxmox-Installation-WiFi-Setup.md)

     2.1. [Install Proxmox VE](sections/Proxmox-Installation-WiFi-Setup.md#11-Install-Proxmox-VE)
     
     2.2. [Getting Temporary Internet Access](sections/Proxmox-Installation-WiFi-Setup.md#12-Getting-Temporary-Internet-Access)

     2.3. [Enable Wi-Fi Client](sections/Proxmox-Installation-WiFi-Setup.md#13-enable-wifi-client-1)

4. [OPNsense Deployment & VLAN Setup]()

5. [Initial Firewall Setup]()

6. Challenges & Conclusion
---

## 


