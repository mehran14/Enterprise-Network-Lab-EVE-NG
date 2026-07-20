# Enterprise Network Lab \[EVE-NG]



A multi-site enterprise network lab implemented in EVE-NG to demonstrate enterprise routing, STP, HSRP, BGP, OSPF, GRE over IPsec, NAT, AAA, SSH security, and network troubleshooting.



This project was designed as a practical preparation lab for senior network engineering and infrastructure interviews.



## 1\. Project Overview



This lab simulates an enterprise network consisting of:



* Two enterprise sites:

  * Headquarters (HQ)
  * Branch
* Two Internet Service Providers:

  * ISP 1 – AS 100
  * ISP 2 – AS 200
* Internal enterprise routing using OSPF
* External routing using eBGP and iBGP
* Secure inter-site connectivity using GRE over IPsec
* Internet access using PAT
* Public access to the internal web server using Static NAT
* Centralized device administration using TACACS+
* Secure management using SSHv2
* Routing and management security controls



## 2\. Lab Objectives



The main objectives of this project were:



* Design and implement a multi-site enterprise topology
* Configure OSPF inside the HQ and Branch sites
* Configured HSRP with IP SLA object tracking for automatic failover
* Advertise organizational loopback interfaces through OSPF
* Secure OSPF adjacencies using authentication
* Configure iBGP between HQ edge routers
* Configure iBGP and eBGP inside the ISP infrastructure
* Establish eBGP peering between enterprise edge routers and ISP routers
* Build a GRE tunnel over IPsec between HQ and Branch
* Exchange inter-site routes through the tunnel
* Configure PAT for internal users
* Configure Static NAT for the HQ web server
* Prevent VPN traffic from being translated
* Secure router management with SSHv2
* Implement TACACS+ authentication and authorization
* Configure local authentication as AAA fallback
* Apply management ACLs and control-plane protection
* Verify connectivity, routing, security, and failover behavior



## 3\. Network Topology



!\[Enterprise-Network-Lab-EVE-NG](topology/enterprise-topology.png)



The topology includes the following components:



#### Headquarters



\- HQ-EDGE1

\- HQ-EDGE2

\- HQ-CORE1

\- HQ-CORE2

\- AAA-SRV-HQ

\- WEB-SRV-HQ

\- MGMT-PC-HQ



#### Branch



\- BR-EDGE1

\- BR-CORE1

\- BR-CORE2

\- MGMT-PC-BR



#### ISP Infrastructure



\- ISP-R1

\- ISP-R2

\- ISP-R3

\- ISP-R4

\- ISP-R5

\- INET-DNS – 8.8.8.8 simulation



## 4\. Autonomous System Design



| Organization | Routers | AS Number | BGP Role |

|-------|----------------|-----|---------------------------------|

| ISP 1 | ISP-R1, ISP-R5 | 100 | Internal iBGP and external eBGP |

| ISP 2 | ISP-R2, ISP-R3, ISP-R4 | 200 | Internal iBGP and external eBGP |

| HQ Enterprise | HQ-EDGE1, HQ-EDGE2 | 65001 | Enterprise edge BGP |

| Branch Enterprise | BR-EDGE1 | 65002 | Branch edge BGP over GRE tunnel |



#### ISP Design Notes



\- ISP 1 operates under AS 100.

\- ISP 2 operates under AS 200.

\- ISP-R4 provides an internal core/transit role inside ISP 2.

\- iBGP is used between routers inside the same autonomous system.

\- eBGP is used between different autonomous systems.

\- Enterprise edge routers establish eBGP sessions with upstream ISP routers.

\- HQ and Branch exchange enterprise routes over the GRE over IPsec tunnel.



## 5\. Routing Design



### 5.1 Internal Routing



OSPF is used as the IGP inside the enterprise sites.



#### HQ



OSPF is enabled between:



\- HQ-EDGE1

\- HQ-EDGE2

\- HQ-CORE1

\- HQ-CORE2



#### Branch



OSPF is enabled between:



\- BR-EDGE1

\- BR-CORE1

\- BR-CORE2



All enterprise loopback interfaces are advertised through OSPF.



### 5.2 OSPF Security



OSPF neighbor relationships are protected using authentication.



Security objectives:



\- Prevent unauthorized OSPF neighbors

\- Prevent false route injection

\- Authenticate routing protocol packets

\- Verify neighbor adjacency using secure credentials



### 5.3 BGP



The following BGP relationships are implemented:



\- iBGP between HQ-EDGE1 and HQ-EDGE2

\- iBGP within ISP 1

\- iBGP within ISP 2

\- eBGP between enterprise edge routers and ISP routers

\- eBGP between HQ and Branch over the GRE tunnel



BGP neighbor authentication is enabled using MD5/TCP authentication.



## 6\. GRE over IPsec Design



A GRE tunnel over IPsec is implemented between:

HQ-EDGE1 ↔ BR-EDGE1



**The GRE tunnel provides:**



* A logical Layer 3 path between HQ and Branch
* Support for routing protocols across the tunnel
* Inter-site route exchange
* Transport for eBGP between HQ and Branch



**IPsec provides:**



* Confidentiality
* Integrity
* Peer authentication
* Secure transport for GRE traffic
* MTU and TCP MSS adjustments are configured to reduce fragmentation across the tunnel



## 7\. NAT Design

### 7.1 PAT

PAT is configured on enterprise edge routers to provide Internet access for internal users.



**Internal users include:**



* HQ management hosts
* Branch management hosts
* Internal enterprise networks



### 7.2 Static NAT

The internal HQ web server is published to the simulated Internet using Static NAT.



WEB-SRV-HQ private address

↓

Public translated address



### 7.3 NAT Exemption

Traffic between HQ and Branch must not be translated.



**The NAT policy excludes:**



* GRE tunnel traffic
* Inter-site enterprise networks
* VPN-related traffic
* Private traffic exchanged between HQ and Branch



## 8\. Management and AAA Design

All routers are configured for secure administrative access.



* Management Security
* SSH version 2 only
* Telnet disabled
* Local administrator account configured
* Management ACL applied to VTY lines
* Only approved management networks are permitted
* SSH access is available between the two enterprise sites
* AAA



\[**Note:** In this version only RADIUS deployed on HQ. TACACS+ server is not available for me in EVE-NG right now.]



A centralized TACACS+ server is deployed at HQ:



AAA-SRV-HQ TACACS+ is used for:



* Administrator authentication
* Command authorization
* Privilege-level authorization
* Centralized access control
* Local authentication is configured as a fallback if the TACACS+ server becomes unavailable.



## 9\. Security Controls

The following security controls are implemented:



* OSPF authentication
* BGP neighbor authentication
* GRE over IPsec encryption
* SSHv2-only management access
* Telnet disabled
* Management ACLs on VTY lines
* Local AAA fallback
* Control-plane protection
* NAT exemption for VPN traffic
* Secure router passwords
* Logging for administrative and routing events
* NTP synchronization where applicable (is not deployed in this version)



## 10\. Repository Structure

.

├── README.md

├── topology/

│   └── enterprise-topology.png

├── configs/

│   ├── hq/

│   ├── branch/

│   └── isp/

├── documentation/

│   ├── ip-addressing-table.md

│   ├── routing-design.md

│   ├── bgp-as-design.md

│   ├── aaa-design.md

│   └── security-controls.md

├── verification/

│   ├── ospf-verification.txt

│   ├── bgp-verification.txt

│   ├── nat-verification.txt

│   ├── vpn-verification.txt

│   └── ssh-aaa-verification.txt

├── screenshots/

└── eve-ng/

&#x20;   └── enterprise-network-lab.unl



## 11\. Useful Verification Commands



##### OSPF



show ip ospf neighbor

show ip ospf interface brief

show ip route ospf

show ip protocols



##### BGP



show ip bgp summary

show ip bgp neighbors

show ip bgp

show ip route bgp



##### GRE and IPsec



show interfaces tunnel

show crypto isakmp sa

show crypto ipsec sa

show crypto session



##### NAT



show ip nat translations

show ip nat statistics

show access-lists



##### SSH and AAA



show ip ssh

show running-config | section aaa

show users

show ssh



##### Connectivity



ping

ping source Loopback0

traceroute

traceroute source Loopback0



## 12\. Configuration Files

Router configurations are available in the following directories:



* HQ Router Configurations
* Branch Router Configurations
* ISP Router Configurations



## 13\. Documentation

Detailed design documents are available in the documentation directory:



* IP addressing table
* Routing design
* BGP AS design
* AAA design
* Security controls



## 14\. Verification Results

Verification outputs are stored in the verification directory.



The verification process covers:



* OSPF neighbor formation
* OSPF route learning
* BGP session establishment
* BGP route exchange
* GRE tunnel status
* IPsec security associations
* Internet connectivity
* PAT translations
* Static NAT access
* SSH connectivity
* RADIUS authentication
* Local AAA fallback
* Inter-site communication



## 15\. How to Use the Lab



1. Deploy the topology in EVE-NG.
2. Import or recreate the required nodes.
3. Apply the router configurations from the configs directory.
4. Configure the server and host IP addresses.
5. Verify OSPF neighbor relationships.
6. Verify BGP sessions and route exchange.
7. Verify the GRE over IPsec tunnel.
8. Test NAT and Internet access.
9. Test SSH and TACACS+ (RADIUS on HQ-EDGE1 for this version) authentication.
10. Review the verification files for expected results.



## 16\. Author

**Name**: Mehran Aghemiri



**Role**: Network Engineering / IT Infrastructure



**Focus Areas**: Cisco Routing, Switching, BGP, OSPF, Network Security, EVE-NG



## 17\. Disclaimer

This project is a laboratory simulation created for learning, technical practice, and interview preparation.



It does not represent a production network deployment.

