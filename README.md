# Packet-Tracer-Project-5.1---DHCP-Servers

# Project Requirements: Multi-Service Enterprise Network Implementation

![Project 2 Screenshot](https://github.com/pinoechalle-lang/Packet-Tracer-Project-5.1---DHCP-Servers/blob/main/Screenshot%202026-10-24%20232450.png?raw=true)

## Requirement:
You are a student tasked with designing and implementing a multi-department network infrastructure using Cisco Packet Tracer that demonstrates VLAN segmentation, Router-on-a-Stick inter-VLAN routing, DHCP services, DNS services, and static routing between routers. The project must be configured and verified using real networking principles and validation commands.

## Topology Requirements

### Department A
1. Create two VLANs:
   - VLAN 2
   - VLAN 3

2. Connect one PC to each VLAN.

3. Configure Router-on-a-Stick on Router A to provide:
   - Default gateway for VLAN 2
   - Default gateway for VLAN 3
   - Inter-VLAN routing

### Department B
1. Create two VLANs:
   - VLAN 4
   - VLAN 5

2. Connect one PC to each VLAN.

3. Configure Router-on-a-Stick on Router B to provide:
   - Default gateway for VLAN 4
   - Default gateway for VLAN 5
   - Inter-VLAN routing

## DHCP Requirements

### DHCP Relay Deployment
1. Deploy a dedicated DHCP Server in VLAN 900.
2. Configure DHCP pools to provide addresses for:
   - VLAN 2
   - VLAN 3
3. Configure Router A as a DHCP Relay Agent using:
   - `ip helper-address`
4. Verify that clients in VLAN 2 and VLAN 3 obtain IP configuration from the centralized DHCP Server.

### Router-Based DHCP Deployment
1. Configure Router B as a DHCP Server.
2. Create DHCP pools for:
   - VLAN 4
   - VLAN 5
3. Configure:
   - Network address
   - Default gateway
   - DNS server information
   - Excluded addresses
4. Verify that VLAN 4 and VLAN 5 clients receive IP configuration directly from Router B.

### DHCP Client Demonstration
1. Configure Router A's VLAN 900 interface as a DHCP client using:

```bash
ip address dhcp
```

2. Verify that Router A dynamically receives an IP address from the DHCP Server.

## DNS Requirements

1. Deploy a DNS Server connected to Router B.
2. Assign the DNS Server a static IP address within the 172.19.20.0/24 network.
3. Enable DNS service.
4. Create at least one A Record.
5. Configure DHCP services to distribute the DNS Server address to clients.
6. Verify name resolution from hosts located in VLAN 2, VLAN 3, VLAN 4, and VLAN 5.

## Routing Requirements

### Static Routing

Configure static routes between Router A and Router B to allow communication among:

- VLAN 2
- VLAN 3
- VLAN 4
- VLAN 5
- VLAN 900
- DNS Server network

All subnetworks must be reachable across routers.

---
## Verification Requirements

The following tests must be successfully completed:

```bash
show running-config | section dhcp
show ip dhcp pool
show ip dhcp binding
show ip dhcp conflict
show ip dhcp database

debug ip dhcp server events
debug ip dhcp server packets
undebug all

show running-config | section ip helper-address
show ip interface brief
show ip route
show ip helper-address
```

### DHCP Troubleshooting Command Reference

#### 1) DHCP Server Router (Router B or dedicated DHCP server)
- `show running-config | section dhcp` — Confirms the DHCP service and pool configuration on the server router.
- `show ip dhcp pool` — Displays the DHCP pool details, including network, default gateway, DNS, and lease range.
- `show ip dhcp binding` — Shows which clients have successfully received IP addresses.
- `show ip dhcp conflict` — Lists any detected duplicate or conflicting IP addresses assigned by DHCP.
- `show ip dhcp database` — Confirms the DHCP lease database is active and recording lease information.
- `debug ip dhcp server events` — Enables live DHCP server event logging to observe server-side activity in real time.
- `debug ip dhcp server packets` — Captures DHCP packet exchange between the server and client to diagnose request/response failures.
- `undebug all` — Stops all active debug output after troubleshooting is complete.

#### 2) DHCP Relay Router (Router A)
- `show running-config | section ip helper-address` — Confirms the router is configured to forward DHCP requests to the correct server.
- `show ip interface brief` — Verifies the interfaces involved in DHCP relay are administratively up and operational.
- `show ip route` — Checks the router has a valid route to the DHCP server subnet.
- `show ip helper-address` — Displays the configured helper address(es), confirming the forwarding destination.

#### 3) Where these are typically used
- DHCP server commands are usually entered on the router that is actually running DHCP, such as Router B.
- DHCP relay commands are usually entered on the router forwarding the client requests, such as Router A.
- The debug commands are used on the DHCP server router when you need to observe DHCP traffic live during a failure.

### DHCP Tests
- VLAN 2 client receives address from DHCP Server.
- VLAN 3 client receives address from DHCP Server.
- VLAN 4 client receives address from Router B.
- VLAN 5 client receives address from Router B.
- Router A receives a dynamic address on the VLAN 900 interface.

### Connectivity Tests
- VLAN 2 can reach VLAN 3.
- VLAN 4 can reach VLAN 5.
- Department A can communicate with Department B.
- All clients can reach the DHCP Server.
- All clients can reach the DNS Server.

### DNS Tests
- Clients obtain DNS server information from DHCP.
- Hostnames resolve to IP addresses.
- Clients can access a host using its domain name instead of its IP address.
