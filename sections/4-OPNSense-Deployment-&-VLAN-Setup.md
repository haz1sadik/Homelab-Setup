## 4. OPNsense Deployment
    
### 4.1 Linux Bridge Setup for WAN & LAN interfaces

Since OPNsense is based on FreeBSD that does not play well with Wi-Fi drivers, we need to set a "virtual wire" to send Proxmox's Wi-Fi internet to OPNsense as our WAN. Explanation can be found in [Techinal Security Decisions](2-Technical-Security-Decisions.md#21-wi-fi-as-nat):

   - In the Proxmox Web GUI, go to System > Network
     
       ![vmbr1 creation](../assets/vmbr1-creation.png)
   - Create a new Linux Bridge `vmbr1`:
       - Assign `172.16.0.1/24` IPv4 [^1]
       - Check the Autostart checkbox
       - Put a descriptive comment
    
   - For `vmbr0` be sure that VLAN aware checkbox is checked

   - Next we have to route the Wi-Fi traffic to `vmbr1` by adding these NAT rules under the Wi-Fi interface in `/etc/network/interfaces`:
       ```bash
          post-up echo > /proc/sys/net/ipv4/ip_forward
          post-up iptables -t nat -A POSTROUTING -s 172.16.0.0/24 -o [interface] -j MASQUERADE
          post-down iptables -t nat -D POSTROUTING -s 172.16.0.0/24 -o [interface] -j MASQUERADE
       ```
       e.g.:
     
       ![NAT rules](../assets/NAT-rules-WAN.png)

### 4.2 Creating OPNsense VM in Proxmox:

   - Login to Proxmox and create a new VM with the following configurations:
     
     - 20GB SCSI disk (OPNsense dont require large disk space)
     - 2 CPU Core
     - 4GB RAM recommended but 2GB is okay for now
     - No network device for now
    
   - Next we will add both our WAN & LAN network device in VM > Hardware:

     - First we add `net0` pointing to `vmbr0` as our LAN
     - Secondly, we add `net01` pointing to `vmbr1` as our WAN

  >[!IMPORTANT]
  >The network device arrangement is **important** as first network device will always be configured as LAN by OPNsense.

### 4.3 OPNsense Installation & Initial Setup

   - Start the created VM
   - Login with username `installer` and password `opnsense` to perform installation
   - After the installation have completed, reboot the VM
 #### 4.3.1 Solving IP conflict with Mercusys IP
   - By default, OPNsense configure LAN to use `192.168.1.1/24` as the management IP. We have to change it as that IP is taken by our Mercusys router:[^2]
     
       - Login as root.
       - Select `2` and press Enter
       - Choose the LAN interface
       - `Configure IPv4 address via DHCP?` Type n
       - `Enter the new LAN IPv4 address:` Type 192.168.1.3
       - `Enter the new LAN IPv4 subnet mask:` Type 24
       - `For WAN/Gateway/IPv6:` Just hit Enter to keep defaults
       - `Do you want to enable the DHCP server on LAN?` Type n for now (We will configure our own DHCP server later)
    
   - Try to access OPNsense Web GUI from the management pc at `192.168.1.3`

#### 4.3.2 Configuring WAN for internet access
   - We need to point our WAN interface to our `vmbr1` in Proxmox:
     
      - Login as root in OPNsense console
      - Select `2` and press Enter
      - Choose the WAN interface
      - `Configure IPv4 via DHCP?` Type n
      - `Enter the new WAN IPv4 address:` 10.254.0.2
      - `Enter the new WAN IPv4 subnet mask:` 24
      - `Enter the IPv4 gateway address:` 10.254.0.1 (This will points to Proxmox)
      - Hit Enter through the remaining prompts
        
  - Try to ping `8.8.8.8`


[^1]: I use `172.16.0.x` range for `vmbr1` to avoid confusion with my home networks
[^2]: Refer the [Network Architecture](1-Network-Architechture.md#1-network-architecture)
