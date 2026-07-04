# Routing and Switching (Security Perspective)

## Introduction

Routing and Switching are two fundamental networking concepts that determine how devices communicate within and across networks.

Although both technologies enable connectivity, they operate at different layers of the TCP/IP stack and expose different security considerations.

From an offensive security perspective, switching defines communication within a local network, while routing determines how traffic moves between different network segments.

Understanding both technologies is essential for reconnaissance, attack path analysis, lateral movement, and network segmentation assessment.

---

# Why Routing and Switching Matter

Enterprise environments rarely consist of a single flat network.

Instead, organizations divide infrastructure into multiple VLANs and subnets connected through switches and routers.

Examples include:

- User Networks
- Server Networks
- Domain Controller Networks
- Management Networks
- DMZ
- Guest Networks

Attackers analyze switching and routing to understand how these segments communicate and where security boundaries exist.

---

# Switching

## Overview

A network switch connects devices within the same Local Area Network (LAN).

Unlike hubs, switches forward Ethernet frames only to the intended destination using MAC addresses.

This improves performance while reducing unnecessary traffic.

Switching primarily operates at Layer 2.

---

## How Switching Works

A switch maintains a MAC Address Table that maps device MAC addresses to physical switch ports.

When a frame arrives:

1. The switch learns the source MAC address.
2. It checks its MAC table.
3. If the destination MAC is known, the frame is forwarded only to the appropriate port.
4. If unknown, the frame is flooded throughout the VLAN.

This learning process occurs automatically.

---

# Switching Security Risks

## ARP Spoofing

Attackers manipulate ARP messages to redirect traffic through their own device.

Potential impact:

- Traffic interception
- Credential capture
- Session hijacking
- NTLM Relay
- Man-in-the-Middle attacks

---

## MAC Flooding

An attacker floods the switch with fake MAC addresses.

When the MAC table becomes full, some switches begin forwarding traffic like a hub.

Potential impact:

- Increased visibility
- Packet sniffing
- Information disclosure

---

## VLAN Hopping

Improper switch configuration may allow traffic to cross VLAN boundaries.

Potential impact:

- Segmentation bypass
- Unauthorized network access
- Expanded attack surface

---

## Rogue Devices

Unauthorized devices connected to unused switch ports may gain access to internal networks.

Without proper port security, physical access often becomes network access.

---

# Routing

## Overview

Routers connect multiple IP networks and determine the path packets take toward their destination.

Unlike switches, routers operate using IP addresses and routing tables.

Routing primarily occurs at Layer 3.

---

## How Routing Works

When a packet arrives:

1. The router examines the destination IP address.
2. The routing table is consulted.
3. The most appropriate route is selected.
4. The packet is forwarded toward the destination network.

Routers therefore define communication between security zones.

---

# Routing Security Risks

## Misconfigured Routing

Incorrect routing rules may expose sensitive internal networks.

Examples include:

- User access to management networks.
- Public access to internal services.
- Unrestricted east-west communication.

---

## Route Manipulation

Compromised routing infrastructure may redirect traffic toward attacker-controlled systems.

Potential consequences include:

- Traffic interception
- Network disruption
- Information disclosure

---

## Weak Network Segmentation

Routing policies determine whether attackers can move between network segments.

Poor segmentation often enables:

- Lateral Movement
- Domain Enumeration
- Privilege Escalation
- Full Enterprise Compromise

---

# Attacker Perspective

Following initial access, attackers seek to understand the organization's network architecture.

Typical objectives include:

- Identify VLANs.
- Discover internal subnets.
- Locate default gateways.
- Enumerate routing paths.
- Find Domain Controllers.
- Detect management networks.
- Identify weak segmentation.

Understanding routing often determines whether additional systems are reachable.

---

# Defensive Perspective

Security teams use routing and switching to enforce network security boundaries.

Common defensive measures include:

- VLAN Segmentation
- Access Control Lists (ACLs)
- Inter-VLAN Firewalls
- Dynamic ARP Inspection
- DHCP Snooping
- Port Security
- Private VLANs
- Zero Trust Network Segmentation

Proper network design significantly limits attacker movement following an initial compromise.

---

# Routing vs Switching

| Feature | Switching | Routing |
|----------|-----------|----------|
| Primary Layer | Layer 2 | Layer 3 |
| Address Used | MAC Address | IP Address |
| Scope | Local Network | Multiple Networks |
| Main Purpose | Deliver Frames | Forward Packets |
| Security Focus | Local Traffic Protection | Network Boundary Enforcement |

---

# Common Misconceptions

Several misconceptions frequently appear:

- Switches automatically prevent Man-in-the-Middle attacks.
- Routing alone provides network security.
- VLANs guarantee isolation.
- Internal routing is inherently trusted.

In practice, routing and switching improve communication, but security depends on proper configuration, segmentation, authentication, and continuous monitoring.

---

# Key Takeaways

- Switching enables communication within a local network using MAC addresses.
- Routing enables communication between different IP networks.
- Attackers analyze both technologies to understand network topology and identify paths for lateral movement.
- Weak switching configurations facilitate traffic interception, while poor routing policies weaken network segmentation.
- Secure enterprise environments rely on properly configured switches, routers, firewalls, and segmentation controls to reduce attack surface and contain compromises.
