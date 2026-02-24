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
