## 1. Network Architecture

### 1.1 Network Addressing Schema

<table>
  <thead>
    <tr>
      <th>Component</th>
      <th>Interface</th>
      <th>IP Address</th>
      <th>Network</th>
      <th>Purpose</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Home Router</strong></td>
      <td>WAN</td>
      <td><code>192.168.0.1</code></td>
      <td>Home Net</td>
      <td>Internet Gateway</td>
    </tr>
    <tr>
      <td><strong>Mercusys Router</strong></td>
      <td><code>Physical</code></td>
      <td><code>192.168.1.1</code></td>
      <td>Management</td>
      <td>Management Access</td>
    </tr>
    <tr>
      <td><strong>Proxmox Host</strong></td>
      <td><code>vmbr0</code></td>
      <td><code>192.168.1.2</code></td>
      <td>Management</td>
      <td>Hypervisor Management</td>
    </tr>
    <tr>
      <td rowspan="3"><strong>OPNSense</strong></td>
      <td><code>vmbr0</code></td>
      <td><code>192.168.1.3</code></td>
      <td>Management</td>
      <td>Access to OPNSense Web Console</td>
    </tr>
    <tr>
      <td><code>vmnet1</code>(WAN)</td>
      <td><code>172.16.0.2</code></td>
      <td>Transit Net</td>
      <td>Upstream to Proxmox NAT</td>
    </tr>
    <tr>
      <td><code>vmnet0</code>(LAN)</td>
      <td><code>10.0.x.1</code></td>
      <td>Trunk</td>
      <td>Lab VLANs Gateway</td>
    </tr>
    <tr>
      <td><strong>Offensive VMs</strong></td>
      <td><code>Ethernet</code></td>
      <td><code>10.0.10.x</code></td>
      <td>VLAN 10</td>
      <td>Attacker Node</td>
    </tr>
    <tr>
      <td><strong>Defensive VMs</strong></td>
      <td><code>Ethernet</code></td>
      <td><code>10.0.20.x</code></td>
      <td>VLAN 20</td>
      <td>Research Node</td>
    </tr>
    <tr>
      <td><strong>Management PC</strong></td>
      <td>Physical</td>
      <td><code>192.168.1.4</code></td>
      <td>Management</td>
      <td>Secure Access to Proxmox / OPNSense</td>
    </tr>
  </tbody>
</table>

### 1.2 Network Topology

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
