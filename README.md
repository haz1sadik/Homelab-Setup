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

1. [Proxmox Installation & Host Preparation](Proxmox-Installation-Host-Prep.md#Proxmox-Installation-&-Host-Preparations)

     1.1. [Install Proxmox VE](Proxmox-Installation-Host-Prep.md#11-Install-Proxmox-VE)
     
     1.2. [Getting Temporary Internet Access](Proxmox-Installation-Host-Prep.md#12-Getting-Temporary-Internet-Access)

     1.3. [Enable Wi-Fi Client](Proxmox-Installation-Host-Prep.md#13-enable-wifi-client-1)

---

## 

```mermaid
graph TD
    subgraph Home_Network [Home Network - 192.168.0.x]
        A[Home Router]
    end

    subgraph Proxmox_Host [Proxmox Host - 192.168.1.2]
        direction TB
        B[WiFi Interface] -- NAT --> C[vmbr1: 172.16.0.1]
        D[Physical Ethernet] --- E[vmbr0: VLAN Aware]
    end

    subgraph OPNsense_Firewall [OPNsense VM - 192.168.1.3]
        C -- WAN --> F[vtnet1: 172.16.0.2]
        E -- LAN Trunk --> G[vtnet0]
        G --- H[VLAN 10: Attacker]
        G --- I[VLAN 20: Victim]
    end

    subgraph Lab_Nodes [Virtual Machines]
        H --- K[Kali Linux - 10.0.10.x]
        I --- V[Windows Target - 10.0.20.x]
    end

    subgraph Physical_Access [Management Plane]
        E --- M[Mercusys Switch]
        M --- PC[Management PC - 192.168.1.x]
    end

    A -. WiFi .-> B
