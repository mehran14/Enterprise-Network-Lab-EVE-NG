# IP Addressing Table



This document contains the consolidated IP addressing plan for the Enterprise Network Lab.



### 1\. Enterprise Loopback Interfaces \& Router IDs



| Device | Interface | IP Address | Purpose |

|---|---|---|---|

| HQ-EDGE1 | Loopback0 | `11.11.11.11/32` | BGP Router ID \& Management |

| HQ-EDGE2 | Loopback0 | `22.22.22.22/32` | BGP Router ID \& Management |

| HQ-CORE1 | Loopback0 | `1.1.1.1/32` | OSPF Router ID |

| HQ-CORE2 | Loopback0 | `2.2.2.2/32` | OSPF Router ID |

| BR-EDGE1 | Loopback0 | `33.33.33.33/32` | BGP Router ID \& Management |

| BR-CORE1 | Loopback0 | `3.3.3.3/32` | OSPF Router ID |

| BR-CORE2 | Loopback0 | `4.4.4.4/32` | OSPF Router ID |



### 2\. HQ Internal Routed Links \& LANs



HQ internal network uses `10.0.0.0/16` for addressing.



| Device | Interface | IP Address | Subnet Mask | Connected to / Description |

|---|---|---|---|---|

| HQ-CORE1 | Gi0/0 | `10.0.0.1` | `/30` | HQ-CORE2 (Gi0/0 - `10.0.0.2`) |

| HQ-CORE1 | Gi0/1 | `10.0.0.5` | `/30` | HQ-EDGE1 (Gi0/1 - `10.0.0.6`) |

| HQ-CORE1 | Gi0/2 | `10.0.0.13` | `/30` | HQ-EDGE2 (Gi0/1 - `10.0.0.14`) |

| HQ-CORE1 | Gi0/3.10 | `10.0.10.2` | `/24` | VLAN 10 - HR LAN (HSRP VIP: `10.0.10.254`) |

| HQ-CORE1 | Gi0/3.20 | `10.0.20.2` | `/24` | VLAN 20 - Servers (HSRP VIP: `10.0.20.254`) |

| HQ-CORE1 | Gi0/3.99 | `10.0.99.2` | `/24` | VLAN 99 - Management (HSRP VIP: `10.0.99.254`) |

| HQ-ASW1 | VLAN 99 | `10.0.99.11` | `/24` | Switch Management (Gateway: `10.0.99.254`) |

| HQ-ASW2 | VLAN 99 | `10.0.99.12` | `/24` | Switch Management (Gateway: `10.0.99.254`) |

| AAA-SRV-HQ | NIC | `10.0.20.10` | `/24` | Centralized AAA / DHCP Server |

| WEB-SRV-HQ | NIC | `10.0.10.50` | `/24` | Web Server (Published to Internet) |



### 3\. Branch Internal Routed Links \& LANs



Branch internal network uses `10.1.0.0/16` for LANs and `10.200.0.0/16` for transit.



| Device | Interface | IP Address | Subnet Mask | Connected to / Description |

|---|---|---|---|---|

| BR-CORE1 | Gi0/1 | `10.200.1.1` | `/30` | BR-CORE2 (Gi0/1 - `10.200.1.2`) |

| BR-CORE1 | Gi0/0 | `10.200.1.5` | `/30` | BR-EDGE1 (Gi0/0 - `10.200.1.6`) |

| BR-CORE1 | Gi0/2.10 | `10.1.10.2` | `/24` | VLAN 10 - Branch Users (HSRP VIP: `10.1.10.1`) |

| BR-CORE1 | Gi0/2.20 | `10.1.20.2` | `/24` | VLAN 20 - Branch LAN (HSRP VIP: `10.1.20.1`) |

| BR-CORE1 | Gi0/2.99 | `10.1.99.2` | `/24` | VLAN 99 - Branch Management (HSRP VIP: `10.1.99.1`) |

| BR-ASW1 | VLAN 99 | `10.1.99.11` | `/24` | Switch Management (Gateway: `10.1.99.1`) |

| BR-ASW2 | VLAN 99 | `10.1.99.12` | `/24` | Switch Management (Gateway: `10.1.99.1`) |



### 4\. ISP \& WAN Edge Interconnections



| Local Device | Local Interface | Local IP | Remote Device | Remote IP | Subnet | AS Relationship |

|---|---|---|---|---|---|---|

| HQ-EDGE1 | Gi0/3 | `198.51.100.1` | ISP-R1 (Gi0/0) | `198.51.100.2` | `198.51.100.0/30` | eBGP (65001 - 100) |

| HQ-EDGE2 | Gi0/3 | `198.51.100.5` | ISP-R2 (Gi0/3) | `198.51.100.6` | `198.51.100.4/30` | eBGP (65001 - 200) |

| BR-EDGE1 | Gi0/3 | `198.51.100.10` | ISP-R3 (Gi0/?) | `198.51.100.9` | `198.51.100.8/30` | eBGP (65002 - 200) |



### 5\. ISP Core Infrastructure (AS 100 \& AS 200)



| Local Device | Interface | Local IP | Remote Device | Remote IP | Subnet | Connection Type |

|---|---|---|---|---|---|---|

| ISP-R1 | Gi0/1 | `100.0.0.1` | ISP-R5 | `100.0.0.2` | `100.0.0.0/30` | iBGP / IGP (AS 100) |

| ISP-R1 | Gi0/3 | `203.0.113.1` | ISP-R2 | `203.0.113.2` | `203.0.113.0/30` | eBGP (AS 100 - AS 200) |

| ISP-R1 | Gi0/2 | `203.0.113.5` | ISP-R4 | `203.0.113.6` | `203.0.113.4/30` | eBGP (AS 100 - AS 200) |

| ISP-R1 | Loopback0 | `100.1.1.1` | N/A | N/A | `/32` | BGP Router ID |

| ISP-R1 | Loopback88 | `8.8.8.8` | N/A | N/A | `/32` | Simulated DNS Server |



### 6\. GRE over IPsec VPN Tunnel



| Device | Tunnel Interface | Tunnel IP | Tunnel Source Interface | Tunnel Source IP | Tunnel Destination |

|---|---|---|---|---|---|

| HQ-EDGE1 | Tunnel100 | `172.16.100.1/30` | Gi0/3 | `198.51.100.1` | `198.51.100.10` (BR-EDGE1) |

| BR-EDGE1 | Tunnel100 | `172.16.100.2/30` | Gi0/3 | `198.51.100.10` | `198.51.100.1` (HQ-EDGE1) |



### 7\. NAT Mapping Table



| NAT Type | Device | Inside (Private) IP | Outside (Public) IP | Port | Purpose |

|---|---|---|---|---|---|

| Static NAT | HQ-EDGE1 | `10.0.10.50` | `198.51.100.1` | TCP 80 | HTTP Web Server Access |

| Static NAT | HQ-EDGE1 | `10.0.10.50` | `198.51.100.1` | TCP 443 | HTTPS Web Server Access |

| Static NAT | HQ-EDGE2 | `10.0.10.50` | `198.51.100.5` | TCP 80 | Redundant HTTP Access |

| Static NAT | HQ-EDGE2 | `10.0.10.50` | `198.51.100.5` | TCP 443 | Redundant HTTPS Access |

| PAT (Overload)| HQ-EDGE1 | `10.0.0.0/16` | Dynamic (Gi0/3 IP) | Any | LAN Internet Access |

| PAT (Overload)| HQ-EDGE2 | `10.0.0.0/16` | Dynamic (Gi0/3 IP) | Any | LAN Internet Access |

| PAT (Overload)| BR-EDGE1 | `10.1.0.0/16` \& `10.200.0.0/16`| Dynamic (Gi0/3 IP) | Any | Branch Internet Access |



