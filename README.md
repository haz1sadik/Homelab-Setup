# My Personal Cybersecurity Homelab Project Setup

---

## Overview

In this project, my main goal is to build and deploy my cybersecurity research lab environment. This project architecture will use **Proxmox VE** as a Type-1 Hypervisor to manage all my virtual machines that will be used in the future project and **OPNSense** as my virtual router and firewall.

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

1. [Network Architecture](sections/1-Network-Architechture.md)

2. [Technical Security Decisions](sections/2-Technical-Security-Decisions.md)

3. [Proxmox Installation & Wi-Fi Setup](sections/3-Proxmox-Installation-WiFi-Setup.md)

     3.1 [Install Proxmox VE](sections/3-Proxmox-Installation-WiFi-Setup.md#31-Install-Proxmox-VE)
     
     3.2 [Getting Temporary Internet Access](sections/3-Proxmox-Installation-WiFi-Setup.md#32-Getting-Temporary-Internet-Access)

     3.3 [Enable Wi-Fi Client](sections/3-Proxmox-Installation-WiFi-Setup.md#33-enable-wifi-client-1)

4. [OPNsense Deployment & VLAN Setup](sections/4-OPNSense-Deployment.md)

     4.1 [Linux Bridge Setup for WAN & LAN interfaces](sections/4-OPNSense-Deployment.md#41-linux-bridge-setup-for-wan--lan-interfaces)

     4.2 [Creating OPNsense VM in Proxmox](sections/4-OPNSense-Deployment.md#42-creating-opnsense-vm-in-proxmox)

     4.3 [OPNsense Installation & Initial Setup](sections/4-OPNSense-Deployment.md#43-opnsense-installation--initial-setup)

     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 4.3.1 [Solving IP conflict with Mercusys IP](sections/4-OPNSense-Deployment.md#431-solving-ip-conflict-with-mercusys-ip)
   
     &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 4.3.2 [Configuring WAN for internet access](sections/4-OPNSense-Deployment.md#432-configuring-wan-for-internet-access)

5. [VLANs Setup](sections/5-VLANs-Setup.md)

     5.1 [Creating VLANs](sections/5-VLANs-Setup.md#51-creating-vlans)
   
     5.2 [Assigning Interface](sections/5-VLANs-Setup.md#52-assigning-interface)
   
     5.3 [Kea DHCP Server Configuration](sections/5-VLANs-Setup.md#53-kea-dhcp-server-configuration)
   
7. [Initial Firewall Setup](sections/6-Initial-Firewall-Setup.md)
   
     6.1 [Alias Definition](sections/6-Initial-Firewall-Setup.md#61-alias-definition)
   
     6.2 [Offensive Rules in VLAN 10](sections/6-Initial-Firewall-Setup.md#62-offensive-rules-in-vlan-10)
   
     6.3 [Defensive Rules in VLAN 20](sections/6-Initial-Firewall-Setup.md#63-defensive-rules-in-vlan-20)

---

## Challenges & Conclusion


