### Networking Basics Overview

### Introduction

Computer networks form the foundation of modern information systems. Every web application, cloud platform, enterprise infrastructure, and Active Directory environment depends on network communication.

For offensive security professionals, networking is more than understanding how devices communicate. It is about understanding how attackers discover systems, move through environments, exploit trust relationships, and interact with services across network boundaries.

A solid understanding of networking is essential before studying web security, Active Directory, cloud environments, or advanced penetration testing methodologies.

---

### What is a Network?

A computer network is a collection of interconnected devices that exchange information using standardized communication protocols.

These devices may include:

- Workstations
- Servers
- Network appliances
- Mobile devices
- IoT devices
- Cloud resources

Communication occurs through protocols that define how data is transmitted, routed, received, and interpreted.

From a security perspective, a network represents multiple trust relationships where data flows between systems that may have different security requirements.

---

### Why Networking Matters in Offensive Security

Nearly every penetration test begins with understanding the target network.

Attackers rely on networking to:

- Discover reachable systems
- Enumerate exposed services
- Identify trust relationships
- Locate authentication services
- Exploit communication protocols
- Perform lateral movement
- Establish command-and-control channels
- Exfiltrate sensitive information

Without understanding network behavior, exploiting vulnerabilities becomes significantly more difficult.

---

### Networking from a Security Perspective

Security professionals analyze networks differently than traditional network administrators.

Instead of asking:

«"How do devices communicate?"»

They ask:

- What systems are reachable?
- Which services are exposed?
- Where are trust boundaries located?
- Which protocols are insecure?
- Which paths allow privilege escalation?
- Which assets are externally accessible?
- What information can be gathered without authentication?

Networking becomes a process of mapping the attack surface rather than simply connecting devices.

---

### Core Networking Concepts

Every penetration tester should understand the following concepts:

Hosts

Any device connected to a network capable of sending or receiving data.

Examples include:

- Workstations
- Servers
- Firewalls
- Printers
- Virtual machines

---

### IP Addressing

IP addresses uniquely identify devices within a network.

Understanding addressing helps identify:

- Network ranges
- Internal infrastructure
- Routing paths
- Segmentation
- Reachable targets

---

### Ports

Ports identify services running on a host.

Examples include:

- HTTP
- HTTPS
- SSH
- SMB
- DNS
- RDP

Open ports define part of the exposed attack surface.

---

### Protocols

Protocols define communication rules between systems.

Examples include:

- TCP
- UDP
- HTTP
- HTTPS
- DNS
- SMB
- LDAP
- Kerberos

Each protocol introduces different security considerations and potential attack vectors.

---

### Trust Boundaries

A trust boundary represents a point where the security context changes.

Examples include:

- Internet → DMZ
- DMZ → Internal Network
- User Network → Domain Controllers
- Corporate Network → Cloud Environment

Attackers often focus on crossing trust boundaries because doing so expands access to additional resources.

---

### Attack Surface

The network attack surface consists of every component that can potentially be interacted with.

Examples include:

- Open ports
- Public services
- VPN gateways
- APIs
- Remote management interfaces
- Domain Controllers
- File servers

Reducing unnecessary exposure directly reduces attack opportunities.

---

### Attacker Perspective

An attacker typically views a network as a collection of opportunities rather than connected devices.

Key objectives include:

- Discover hosts
- Identify live services
- Enumerate technologies
- Detect misconfigurations
- Identify authentication mechanisms
- Locate sensitive assets
- Build attack paths
- Expand access through trusted relationships

Network reconnaissance is therefore the first phase of nearly every offensive security engagement.

---

### Defensive Perspective

Defenders analyze networks differently.

Their priorities include:

- Network segmentation
- Least privilege communication
- Traffic monitoring
- Secure protocol configuration
- Firewall policy enforcement
- Intrusion detection
- Asset visibility
- Attack surface reduction

Effective defense limits attacker visibility and restricts lateral movement.

---

### Common Misconceptions

Some common misconceptions include:

- Networking is only important for network engineers.
- Knowing IP addresses is sufficient for penetration testing.
- Open ports always indicate vulnerabilities.
- Internal networks are inherently trusted.
- Firewalls alone provide adequate protection.

In practice, attackers exploit weak trust relationships, protocol misuse, and configuration errors more frequently than networking technologies themselves.

---

### Summary

Networking is the foundation upon which every offensive security discipline is built.

Understanding how systems communicate enables penetration testers to discover attack surfaces, analyze trust relationships, identify exposed services, and construct realistic attack paths.

Strong networking knowledge transforms isolated technical findings into complete attack chains that accurately represent real-world adversary behavior.

---

### Next Topics

The next chapters build upon these concepts by exploring:

- TCP/IP Model
- IP Addressing
- Subnetting
- ports and services
- common protocols 
- DNS resolution 
- ARP
- Routing and Switching
- network traffic analysis 
- Common Network Protocols
- Network Security basics 
- tools introduction 
- cheatsheet
