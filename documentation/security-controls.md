# Security Controls



### 1\. Routing Protocol Security



##### OSPF



* OSPF authentication is enabled.
* Unauthorized OSPF neighbors are prevented.
* Routing interfaces are explicitly defined.
* Passive interfaces are used where appropriate.



##### BGP



* BGP neighbor authentication is enabled using TCP MD5.
* Only expected prefixes are advertised.
* Neighbor relationships are explicitly configured.
* Prefix filtering is applied where appropriate.
* Maximum-prefix protection can be enabled on external sessions.



### 2\. Management Security



* SSHv2 is used for remote administration.
* Telnet is disabled.
* VTY access is restricted using standard or extended ACLs.
* Only approved management networks are permitted.
* Local administrative fallback is configured.
* Privileged access is protected with encrypted credentials.



### 3\. AAA Security



* TACACS+ (RADIUS for now) is used for centralized authentication.
* TACACS+ is used for command authorization.(will be deployed in future versions)
* Local authentication is used only as a fallback.
* Administrative access attempts can be logged centrally.



### 4\. VPN Security



* GRE is protected with IPsec.
* IKE authentication is configured.
* IPsec encryption and integrity algorithms are explicitly defined.
* VPN peer addresses are restricted.
* Inter-site traffic is excluded from NAT.



### 5\. NAT Security



* Internal private addresses are translated using PAT.
* The HQ web server is published using Static NAT.
* VPN and inter-site traffic is excluded from translation.
* NAT rules are limited to the required source and destination networks.



### 6\. Control-Plane Protection



Control-plane protection is used to limit unwanted traffic destined for the router itself.



**The policy should protect:**



* Routing protocol packets
* SSH management traffic
* ICMP directed at infrastructure addresses
* BGP TCP/179 traffic
* IKE and IPsec traffic



**Verification commands:**



show policy-map control-plane

show policy-map control-plane input

show access-lists



### 7\. Logging and Time Synchronization



**Where applicable:**



* Syslog is configured for important system events.
* NTP is configured for consistent timestamps.
* Login, configuration, routing, and security events are logged.



**Useful commands:**



show logging

show ntp associations

show clock



### 8\. Security Verification



show ip ssh

show access-lists

show crypto isakmp sa

show crypto ipsec sa

show ip bgp neighbors

show ip ospf interface

show policy-map control-plane

show logging









