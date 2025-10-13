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


Static Route Summary
Router	Destination Network	Next Hop Address
R0	2001:DB8:1:2::/64	2001:DB8:3:1::2
R0	2001:DB8:3:2::/64	2001:DB8:3:1::2
R1	2001:DB8:1:1::/64	2001:DB8:3:1::1
R1	2001:DB8:1:2::/64	2001:DB8:3:2::1
R2	2001:DB8:1:1::/64	2001:DB8:3:2::2
R2	2001:DB8:3:1::/64	2001:DB8:3:2::2


 Testing Commands
From R0
bash
ping ipv6 2001:DB8:3:2::1
ping ipv6 2001:DB8:1:2::10
traceroute ipv6 2001:DB8:1:2::10
From PC1
bash
ping ipv6 2001:DB8:1:2::10
From PC2
bash
ping ipv6 2001:DB8:1:1::10


Success Criteria
All routers can ping each other’s interfaces

PC1 and PC2 can communicate across the network

Static routes are correctly configured and verified

Neighbor discovery and routing tables show expected entries
