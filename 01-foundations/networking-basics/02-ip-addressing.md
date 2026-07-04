# IP Addressing (Security Perspective)

## Introduction

IP addressing is one of the fundamental concepts in computer networking. Every device connected to a network requires an IP address to communicate with other systems.

From an offensive security perspective, IP addresses are more than identifiers—they define network visibility, influence attack surface discovery, and help attackers understand how systems are organized within an infrastructure.

Understanding IP addressing is essential for reconnaissance, network enumeration, lateral movement, and attack path analysis.

---

## Why IP Addressing Matters

Nearly every penetration test begins with identifying IP addresses.

Attackers use IP information to:

- Discover reachable hosts
- Identify network boundaries
- Detect internal infrastructure
- Locate critical servers
- Map enterprise environments
- Build attack paths
- Identify segmentation weaknesses

Without IP addressing, network reconnaissance would not be possible.

---

# Public IP Addresses

## Overview

A public IP address is globally routable and accessible over the Internet.

Organizations typically expose public IP addresses for services such as:

- Web Servers
- VPN Gateways
- Email Servers
- DNS Servers
- Remote Access Services
- Cloud Applications

These systems often represent the organization's external attack surface.

---

## Security Risks

Because public IPs are Internet-accessible, they are continuously targeted by attackers.

Common risks include:

- Automated Internet scanning
- Service enumeration
- Vulnerability exploitation
- Password attacks
- Distributed Denial-of-Service (DDoS)
- Banner grabbing
- Version fingerprinting

Proper firewall configuration and service hardening are essential to reduce exposure.

---

# Private IP Addresses

## Overview

Private IP addresses are used within internal networks and are not directly routable over the public Internet.

The reserved private address ranges are:

```text
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
```

These ranges are commonly used in enterprise environments, home networks, and cloud infrastructures.

---

## Security Perspective

Private addressing does not provide security.

Once an attacker gains an initial foothold inside the network, private IP ranges become the primary environment for:

- Internal reconnaissance
- Service discovery
- Active Directory enumeration
- Privilege escalation
- Lateral movement

Many large-scale compromises occur entirely within private address spaces.

---

# Static and Dynamic Addressing

## Static IP Addresses

Static addresses remain constant over time.

They are commonly assigned to:

- Domain Controllers
- File Servers
- Database Servers
- Firewalls
- Network Appliances

### Security Considerations

Because their addresses rarely change, static systems become predictable targets during reconnaissance.

Attackers often prioritize static hosts because they typically provide critical services.

---

## Dynamic IP Addresses

Dynamic addresses are automatically assigned using DHCP.

They are commonly used for:

- User Workstations
- Laptops
- Mobile Devices
- Guest Networks

### Security Considerations

Dynamic addressing introduces different security challenges, including:

- Rogue DHCP Servers
- DHCP Spoofing
- IP Address Reuse
- Asset Tracking Difficulties

Security monitoring should rely on host identity rather than IP address alone.

---

# Internal Network Discovery

After gaining initial access, attackers frequently analyze IP addressing to understand the environment.

Typical objectives include:

- Identifying subnet boundaries
- Locating servers
- Detecting management networks
- Finding Domain Controllers
- Discovering backup infrastructure
- Mapping trust relationships

This information becomes the foundation for lateral movement.

---

# Attacker Perspective

From an attacker's viewpoint, IP addresses reveal valuable information about network architecture.

They help answer questions such as:

- Which systems are reachable?
- Which subnets exist?
- Which hosts appear to be critical?
- Where are authentication services located?
- Which systems communicate frequently?
- Which trust boundaries separate network segments?

IP addressing is therefore one of the first sources of intelligence during reconnaissance.

---

# Defensive Perspective

Defenders use IP addressing to:

- Implement network segmentation
- Control access through firewalls
- Monitor traffic flows
- Detect unauthorized communications
- Restrict lateral movement
- Enforce network policies

Well-designed IP allocation simplifies visibility, monitoring, and incident response.

---

# Common Misconceptions

Several misconceptions are common:

- Private IP addresses are secure by default.
- Hidden IP addresses cannot be discovered.
- Changing IP addresses prevents attacks.
- Internal networks are trusted environments.

In reality, attackers routinely enumerate internal address spaces after obtaining initial access.

---

# Key Takeaways

- IP addresses determine how systems communicate across networks.
- Public IP addresses define the external attack surface.
- Private IP addresses become the primary target after initial compromise.
- IP addressing enables reconnaissance, enumeration, and attack path analysis.
- Proper segmentation and network visibility are more important than simply using private address ranges.
