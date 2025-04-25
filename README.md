# CSE-307-Project-2
# README

## Overview  
This project presents a network design for **IT Solutions**, a mid-sized enterprise with a 5-floor office building. The design ensures efficient communication, scalability, and fault tolerance by combining star and ring topologies, VLSM-based IP addressing, and dynamic routing (RIP v2).

## Network Design Requirements

1. **Topology Selection**  
   - **Floor 1, Floor 2 & Floor 3:** Star topology (central switch per floor connecting 7 PCs + 1 server)  
   - **Floor 4 & Floor 5:** Ring topology (7 PCs on a logical ring; Floor 5 also includes two servers on the ring)  

2. **IP Addressing Scheme** (VLSM, unique per floor)  
   - **Floor 1 (Class B Public):**  
     - **Network:** 131.100.0.0/28  ‣ Mask 255.255.255.240  
     - **Host Range:** 131.100.0.2 – 131.100.0.14  
     - **Broadcast:** 131.100.0.15  
     - **Default Gateway:** 131.100.0.1  
     - **FTP Server:** 131.100.0.2  
   - **Floor 2 (Class B Public):**  
     - **Network:** 131.101.0.0/28  ‣ Mask 255.255.255.240  
     - **Host Range:** 131.101.0.2 – 131.101.0.14  
     - **Broadcast:** 131.101.0.15  
     - **Default Gateway:** 131.101.0.1  
     - **DHCP Server:** 131.101.0.2  
   - **Floor 3 (Class A Private):**  
     - **Network:** 10.0.1.0/28  ‣ Mask 255.255.255.240  
     - **Host Range:** 10.0.1.2 – 10.0.1.14  
     - **Broadcast:** 10.0.1.15  
     - **Default Gateway:** 10.0.1.1  
     - **DNS Server:** 10.0.1.2  
   - **Floor 4 (Class A Private):**  
     - **Network:** 10.0.2.0/28  ‣ Mask 255.255.255.240  
     - **Host Range:** 10.0.2.2 – 10.0.2.14  
     - **Broadcast:** 10.0.2.15  
     - **Default Gateway:** 10.0.2.1  
   - **Floor 5 (Class A Private):**  
     - **Network:** 10.0.3.0/28  ‣ Mask 255.255.255.240  
     - **Host Range:** 10.0.3.2 – 10.0.3.14  
     - **Broadcast:** 10.0.3.15  
     - **Default Gateway:** 10.0.3.1  
     - **Email Server:** 10.0.3.2  
     - **HTTP Server:** 10.0.3.3  

3. **Routing Strategy**  
   - **Protocol:** RIP v2 (dynamic routing across all inter-router links, auto-summarization disabled)  
   - **Routers (4 total):**  
     - **R1:** uplinks Floor 1  
     - **R2:** uplinks Floor 2  
     - **R3:** uplinks Floor 3  
     - **R4 (core):** uplinks Floors 4 & 5 and interconnects to R1/R2/R3  
   - **Inter-Router Links (all /30):**  
     - **R1 ↔ R4:** 192.168.100.0/30  DG on R1 side 192.168.100.1  
     - **R2 ↔ R4:** 192.168.100.4/30  DG on R2 side 192.168.100.5  
     - **R3 ↔ R4:** 192.168.100.8/30  DG on R3 side 192.168.100.9  

## Implementation Details  
- **Devices:** 4 Routers (R1–R4), 5 Switches (one per floor), end-host PCs, and floor servers  
- **Floor 1–3:** PCs & server → switch → local router interface (star)  
- **Floor 4–5:** PCs (and servers on 5) wired in ring; one device acts as gateway to R4  
- **Servers:** FTP on Floor 1; DHCP on Floor 2; DNS on Floor 3; Email & HTTP on Floor 5  
- **VLSM** ensures minimal wasted addresses and clear subnet boundaries  
- **RIP v2** provides automatic route propagation; core router R4 aggregates and distributes routes  
- **Default Gateways:** one per floor (listed above) for client routing to inter-floor network  
