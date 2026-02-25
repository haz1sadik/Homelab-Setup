## 5 VLANs Setup

For VLANs setup, we will create 2 VLANs as follows:

1. **VLAN 10:** Offensive Machines
2. **VLAN 20:** Defensive Machines

### 5.1 Creating VLANs

Since `vtnet0` is connected to our VLAN-aware bridge `vmbr0`, `vtnet0` will be our parent.

   - Navigate to **Interfaces > Devices > VLAN** in OPNsense Web Console
   - Add a new VLAN:
     
       - Device: `vlan0.10`
       - Parent: `vtnet0`
       - VLAN tag; `10`
       - VLAN priority: `Best Effort`
       - Description: `OFFENSIVE`
         
   - Click **Save**
   - Repeat to create another VLAN with tag 20 for Defensive Machines

### 5.2 Assigning Interface

After creating the VLAN interface we need to assign it and enable it with dedicated IP address.

   - Navigate to **Interfaces > Assignments**
   - Under **Assign a new interface** section:
       - Select the VLAN device
       - Fill in the Description
   - Click Add
   - The VLAN will appear at the top section.
   - Click the VLAN interface:

       - Check the `Enable interface` checkbox
       - `IPv4 Configuration Type:` Static IPv4
       - `IPv4 address:` 10.0.10.1/24
         
   - Click **Save**

>[!IMPORTANT]
>Be sure to click on Apply Changes when prompt to for the changes to take effect immediately

### 5.3 Kea DHCP Server Configuration

In order for our VMs to get an IP address, we need to configure a DHCP server for both of our VLANs.

   - Navigate to **Services > Kea DHCP > Control Agent**
   - Enable the control agent and click **Apply**
     
   - Then navigate to Kea **DHCPv4**
   - Enable the service
   - Select both of the VLANs in the Interfaces dropdown
   - Next, go to **Subnets** tab and create a new subnet:
     
       - **Subnet:** 10.0.10.0/24
       - **Description:**** OFFENSIVE
       - **Pools:** 10.0.10.100 - 10.0.10.200
       - **Routers:** 10.0.10.1
       - **DNS servers:** 1.1.1.1
       - Click on **Save**
    
   - Repeat for Defensive Subnet

Now when we create a new VM in Proxmox and assign VLAN tag of 10, Kea DHCP will assign a new IP address in the 10.0.10.x pools
