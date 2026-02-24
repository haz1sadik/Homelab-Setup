## Proxmox Installation & Host Preparations
### 1.1 Install Proxmox VE:
   - Obtain the Proxmox VE ISO from [Official Website](https://www.proxmox.com/en/downloads/proxmox-virtual-environment) and create a bootable USB drive.
   - Perform a standard installation in the host machine.
   - Assign the correct Ethernet port a static IP of `192.168.1.2/24`

### 1.2 Getting Temporary Internet Access:
   - Since my host machine is connected to my Mercusys router that do not have internet access, i need to use USB Tethering from my phone to get temporary internet access.
   - After turning on the USB Tethering, plug in the phone to the host machine.
   - Then run `ip a` to list out all network interfaces:
   
      ![usb tethering interface](assets/usb-tethering-interface.png)
   
     Take note of the network interface name of the USB Tethering device. In my case it is `enxaedde6bf213a`
   - Start the interface and obtain the ip address:
       ```bash
          ip link set <interface> up
          dhclient <interface>
       ```
   - Next we have to set the default gateway to send all outbounds traffic for networks outside of our LAN using IP address we obtain from last command:
       ```bash
           ip route del default
           ip route set default via <IP> dev <interface>
       ```
   - Try to ping 8.8.8.8, if success means we got internet access.

### 1.3 Enable WiFi Client: [^1]
   - Since Debian do not have out-of-box support for Wi-Fi, we need to install the additional driver and tools:
       ```bash
          apt update && apt install wpasupplicant
       ```
   - Create `/etc/wpa_supplicant/wpa_supplicant.conf` and run this command to add your Wi-Fi credentials:
       ```bash
          wpa_passphrase [ssid] [your_password] > /etc/wpa_supplicant/wpa_supplicant.conf
       ```
   - Then edit the `wpa_supplicant.conf` file to include the following:
       ```bash
          ctrl_interface=/run/wpa_supplicant
          update_config=1
          country=[your_country_tag]
          network={
                  ssid="[ssid]"
          #       psk="[your_password]"
                  psk="password_hash"
                  proto=RSN WPA
                  key_mgmt=WPA-PSK
          }
       ```
     If your Wi-Fi is using WPA3-Personal, add `SAE` in `key_mgmt` and the following under the network object:
       ```bash
          sae_password="[your_password]"
          group=CCMP
          mesh_fwding=1
          ieeee80211w=2
       ```
   - Next, we will enable IPv4 forwarding so our Proxmox will act as the gateway for its VMs by running this command:
       ```bash
          echo "net.ipv4.ip_forward=1" >> /etc/sysctl.conf
          sysctl -p
       ```
   - That is it for wpa supplicant configuration. Next we have to configure our Wi-Fi interface in Proxmox by editing the `/etc/network/interfaces` file as follows:
       ```bash
          auto [interface]
          iface [interface] inet dhcp
                wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
       ```
   - After rebooting, try running `wpa_cli status` command to see if Wi-Fi connection is completed and we got an IP address.

      ![wifi connection completed](assets/wifi-connection-completed.png)

   - Try to ping `8.8.8.8` to test for internet connection.

[^1]: Thanks to [this guide](https://github.com/ThomasRives/Proxmox-over-wifi) made by [@ThomasRives](https://github.com/ThomasRives) on how to configure Wi-Fi connection in Proxmox
