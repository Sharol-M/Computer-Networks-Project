# Computer-Networks-Project
Project Description
This project demonstrates a fully functional IPv6 static routing topology using Cisco routers and PCs. It consists of three routers (R0, R1, R2), each connected to a LAN segment with PCs. The routers are interconnected via point-to-point links, and static routes are manually configured to enable end-to-end communication across the network.

The goal is to achieve full IPv6 connectivity between all LANs and router interfaces, validate routing logic, and troubleshoot any asymmetric or unreachable paths using CLI diagnostics like ping, traceroute, and show ipv6 route.

 Topology Overview
Code
PC1 --- R0 --- R1 --- R2 --- PC2
Router 0 (R0) connects to PC1 via LAN1

Router 1 (R1) acts as the transit router between R0 and R2

Router 2 (R2) connects to PC2 via LAN2

IPV6 Addresses

| Device | Interface          | IPv6 Address                    | Purpose          |
| ------ | ------------------ | ------------------------------- | ---------------- |
| R0     | GigabitEthernet0/0 | 2001:DB8:3:1::1/64              | Link to R1       |
| R0     | GigabitEthernet0/1 | 2001:DB8:1:1::1/64              | LAN for PC1      |
| R1     | GigabitEthernet0/0 | 2001:DB8:3:2::2/64              | Link to R2       |
| R1     | GigabitEthernet0/1 | 2001:DB8:3:1::2/64              | Link to R0       |
| R2     | GigabitEthernet0/0 | 2001:DB8:3:2::1/64              | Link to R1       |
| R2     | GigabitEthernet0/1 | 2001:DB8:1:2::1/64              | LAN for PC2      |
| PC1    | NIC                | 2001:DB8:1:1:202:16FF:FE18:5893 | Host in R0’s LAN |
| PC2    | NIC                | 2001:DB8:1:1:250:FFF:FE13:50DA  | Host in R0’s LAN |
| PC3    | NIC                | 2001:DB8:1:2:2D0:BCFF:FE63:D7BD | Host in R2’s LAN |
| PC4    | NIC                | 2001:DB8:1:2:260:47FF:FED3:9282 | Host in R2’s LAN |



Static ipv6 Routing
| Router | Destination Network | Next Hop Address | Purpose                        |
| ------ | ------------------- | ---------------- | ------------------------------ |
| R0     | 2001:DB8:1:2::/64   | 2001:DB8:3:1::2  | Route to R2’s LAN via R1       |
| R0     | 2001:DB8:3:2::/64   | 2001:DB8:3:1::2  | Route to R2’s interface via R1 |
| R1     | 2001:DB8:1:1::/64   | 2001:DB8:3:1::1  | Route to R0’s LAN via R0       |
| R1     | 2001:DB8:1:2::/64   | 2001:DB8:3:2::1  | Route to R2’s LAN via R2       |
| R2     | 2001:DB8:1:1::/64   | 2001:DB8:3:2::2  | Route to R0’s LAN via R1       |
| R2     | 2001:DB8:3:1::/64   | 2001:DB8:3:2::2  | Route to R0’s interface via R1 |



 Full Configuration for Router 0 (R0)
bash
enable
configure terminal
hostname R0

ipv6 unicast-routing

interface GigabitEthernet0/0
 description Link to R1
 ipv6 address 2001:DB8:3:1::1/64
 no shutdown

interface GigabitEthernet0/1
 description LAN for PC1
 ipv6 address 2001:DB8:1:1::1/64
 no shutdown

ipv6 route 2001:DB8:1:2::/64 2001:DB8:3:1::2
ipv6 route 2001:DB8:3:2::/64 2001:DB8:3:1::2

end
write memory





 Testing Commands
 
From R0


ping ipv6 2001:DB8:3:2::1

ping ipv6 2001:DB8:1:2::10

traceroute ipv6 2001:DB8:1:2::10

From PC1


ping ipv6 2001:DB8:1:2::10

From PC2


ping ipv6 2001:DB8:1:1::10


Success Criteria


All routers can ping each other’s interfaces


PC1 and PC2 can communicate across the network


Static routes are correctly configured and verified


Neighbor discovery and routing tables show expected entries


Packets can move from one LAN to another

Hybrid-Configuration-
🌐 Hybrid Network Topology – Cisco Packet Tracer Project

📘 Overview

This project demonstrates the design and implementation of a hybrid enterprise network topology in Cisco Packet Tracer, integrating five fundamental network architectures—Ring, Bus, Mesh, Star, and Extended Star—into a unified environment.

The network employs dual-stack (IPv4/IPv6) configuration, VLAN segmentation, Router-on-a-Stick inter-VLAN routing, and centralized network services (DHCP, DNS, and HTTP). The project also integrates foundational network security measures to emulate a scalable, secure enterprise deployment.

🎯 Learning and Technical Objectives

Design and implement a hybrid topology combining multiple LAN architectures.

Configure IPv4/IPv6 dual-stack connectivity for modern interoperability.

Implement VLANs for logical segmentation and broadcast domain control.

Configure Router-on-a-Stick for inter-VLAN routing.

Deploy centralized DHCP, DNS, and HTTP servers.

Enforce basic network security through device hardening and secure remote access (SSH).

🧩 Network Architecture

⚙️ Topology Components

Component	Quantity	Function
Router	1	Core routing and inter-VLAN gateway
Distribution Switch	1	VLAN trunking and aggregation layer
Access Switches	17	Topology-specific switching
Server	1	DHCP, DNS, HTTP services
End Devices (PCs)	17	Client workstations in each topology
🕸 Topology Breakdown

Ring Topology (VLAN 20)

4 interconnected switches forming a ring

3 PCs distributed across nodes

Provides redundancy and fault tolerance

Bus Topology (VLAN 30)

4 switches in a daisy-chain linear configuration

3 PCs connected to bus segments

Simplified and cost-efficient structure

Mesh Topology (VLAN 40)

5 switches with multiple redundant links

3 PCs across mesh nodes

Ensures high fault tolerance and link diversity

Star Topology (VLAN 50)

1 central switch connecting 4 PCs

Centralized control and easy management

Extended Star Topology (VLAN 60)

Hierarchical structure with 3 switches

4 PCs distributed across layers

Supports scalability and simplified troubleshooting

🌍 IP Addressing Scheme IPv4 Configuration

🌍 IP Addressing Scheme
IPv4 Configuration
VLAN	Network	Subnet Mask	Gateway	DHCP Range	Description
10	192.168.10.0/24	255.255.255.0	192.168.10.1	192.168.10.20–70	Management/Server
20	192.168.20.0/24	255.255.255.0	192.168.20.1	192.168.20.10–60	Ring Network
30	192.168.30.0/24	255.255.255.0	192.168.30.1	192.168.30.10–60	Bus Network
40	192.168.40.0/24	255.255.255.0	192.168.40.1	192.168.40.10–60	Mesh Network
50	192.168.50.0/24	255.255.255.0	192.168.50.1	192.168.50.10–60	Star Network
60	192.168.60.0/24	255.255.255.0	192.168.60.1	192.168.60.10–60	Extended Star
IPv6 Configuration
VLAN	IPv6 Network	Gateway	Description
10	2001:DB8:10::/64	2001:DB8:10::1	Management/Server
20	2001:DB8:20::/64	2001:DB8:20::1	Ring Network
30	2001:DB8:30::/64	2001:DB8:30::1	Bus Network
40	2001:DB8:40::/64	2001:DB8:40::1	Mesh Network
50	2001:DB8:50::/64	2001:DB8:50::1	Star Network
60	2001:DB8:60::/64	2001:DB8:60::1	Extended Star
🧠 Configuration Highlights			
🖧 Core Router – Cisco 1941			
Hostname: Core-Router Function: Layer 3 routing and inter-VLAN communication

Key Features:

IPv4 and IPv6 routing enabled (ipv6 unicast-routing)

802.1Q subinterfaces for VLAN encapsulation

DHCP relay configuration using ip helper-address

Secure management access (SSH, encrypted passwords)

🔀 Distribution Switch – Cisco 2960-24TT

Hostname: Dist-SW Function: VLAN trunking and central distribution layer

Configurations:

VLAN creation (10, 20, 30, 40, 50, 60)

Trunk link to router (Gi0/1)

Trunk uplinks to all topology access switches (Fa0/2–7)

Access port for server (Fa0/1 → VLAN 10)

🖥 Server Configuration

Hostname: Server0 IPv4: 192.168.10.10/24 IPv6: 2001:DB8:10::10/64

Services Deployed:

DHCP Server: 6 address pools for each VLAN

DNS Server: Internal domain mycompany.com

HTTP Server: Basic enterprise web service

📁 Project Structure
Hybrid-Network-Topology/
├── README.md
├── Topology_Diagram.png
├── CiscoPacketTracer_File.pkt
├── Configurations/
│   ├── Core-Router.txt
│   ├── Dist-SW.txt
│   └── VLAN_Config.txt
└── Documentation/
    └── Network_Report.pdf
🔒 Security Implementations

SSH access for secure remote device management

Console and VTY password protection

Service banners for legal notice

DHCP snooping and port security (on access switches)

🧩 Skills Demonstrated

Advanced Cisco IOS configuration (Layer 2 & 3)

IPv4/IPv6 dual-stack design and subnetting

VLAN segmentation and Router-on-a-Stick routing

DHCP, DNS, HTTP service deployment

Network documentation & topology design

Troubleshooting and testing using Packet Tracer simulation tools

📚 Tools & Technologies

Cisco Packet Tracer 8.x

Cisco IOS (Router 1941, Switch 2960)

IPv4 / IPv6 Protocols

DHCP, DNS, HTTP Services

SSH, VLANs, Inter-VLAN Routing
