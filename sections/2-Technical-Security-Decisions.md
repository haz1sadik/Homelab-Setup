## 2. Technical Security Decisions

### 2.1 Wi-Fi as NAT
**Decision:** My Proxmox host will act as a NAT gateway for the OPNSense VM
**Reason:** Proxmox, Linux bridges does not support Wi-Fi client auth due to 4-address frame limitation so by performing NAT on the Proxmox host, we can provide OPNSense a "Virtual Ethernet" WAN interface. Thus, OPNSense and the VMs will have access to internet.

### 2.2 801.1Q VLANs Logical Segmentation
**Decision:** All network traffic between VMs is through VLAN-aware bridge of `vmbr0`
**Reason:** This allows my single physical cable to carry multiple isolated subnet traffics.

### 2. 3 Network Sandboxing
**Decision:** 192.168.1.x subnet is dedicated only for Management Plane (Proxmox & OPNSense Web Console) separate from the Data Plane which is the lab traffic
**Reason:** Even if any of the Defensive VM gets infected with any malware or attack performed by Offensive VM, it cannot scan the network because they are on a different subnet. Thus, protecting my Proxmox Host and my Home network.
