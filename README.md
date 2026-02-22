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

## Proxmox Installation
1. **Install Proxmox VE:**
   - Obtain the Proxmox VE ISO from [Official Website](https://www.proxmox.com/en/downloads/proxmox-virtual-environment) and create a bootable USB drive.
   - Perform a standard installation in the host machine.
   - Assign the correct Ethernet port a static IP of `192.168.1.2/24`

2. **Getting Temporary Internet Access:**
   - Since my host machine is connected to my Mercusys router that do not have internet access, i need to use USB Tethering from my phone to get temporary internet access.
   - After turning on the USB Tethering, plug in the phone to the host machine.
   - Then run `ip a` to list out all network interfaces:
   ![usb tethering interface](assets/usb-tethering-interface.png)
take note of the network interface name of the USB Tethering device. In my case it is `enxaedde6bf213a`
   - Start the interface and obtain the ip address:
   ```bash
      ip link set enxaedde6bf213a up
      dhclient enxaedde6bf213a
   ```

3. **Enable Wi-Fi Client:**
   - Since Debian do not have out-of-box support for Wi-Fi, we need to install the additional driver and tools:
```bash
apt update && apt install wpasupplicant
```
