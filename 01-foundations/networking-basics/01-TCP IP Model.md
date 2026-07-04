# TCP/IP Model (Security Perspective)

### Introduction

The TCP/IP model is the fundamental communication architecture used by modern computer networks. It defines how data is encapsulated, transmitted, routed, and processed between devices across local and global networks.

Unlike the OSI model, which is primarily conceptual, the TCP/IP model reflects the protocols and communication mechanisms implemented in real-world systems.

From an offensive security perspective, every layer of the TCP/IP model introduces its own attack surface, security assumptions, and potential weaknesses that attackers may exploit.

---

### Why the TCP/IP Model Matters

Every penetration test involves network communication.

Whether performing reconnaissance, exploiting a web application, moving laterally inside an enterprise, or exfiltrating data, attackers rely on the protocols defined within the TCP/IP model.

Understanding these layers allows penetration testers to:

- Identify exposed attack surfaces.
- Understand protocol behavior.
- Detect insecure communication.
- Recognize trust relationships.
- Analyze network traffic.
- Map realistic attack paths.

---

### Layer 1 — Network Interface Layer

#### Purpose

The Network Interface Layer manages communication between devices connected to the same physical or wireless network.

Typical technologies include:

- Ethernet
- Wi-Fi
- ARP
- MAC Addressing

This layer is responsible for delivering frames within the local network.

---

### Common Attack Surface

Attackers targeting this layer often attempt to manipulate local network communication.

Common attacks include:

- MAC Address Spoofing
- ARP Spoofing
- ARP Poisoning
- Rogue Access Points
- Evil Twin Attacks
- DHCP Spoofing
- Network Sniffing

Because these attacks occur locally, they often require Layer 2 access to the network.

---

### Security Considerations

Defensive measures include:

- Dynamic ARP Inspection
- Port Security
- 802.1X Authentication
- VLAN Segmentation
- Secure Wi-Fi Configuration

---

### Layer 2 — Internet Layer

#### Purpose

The Internet Layer is responsible for logical addressing and routing packets between different networks.

Key protocols include:

- IPv4
- IPv6
- ICMP
- IP Routing

This layer determines how packets travel from source to destination.

---

### Common Attack Surface

Typical attacks include:

- IP Spoofing
- ICMP Abuse
- Route Manipulation
- Packet Fragmentation Abuse
- Network Scanning
- Host Discovery
- Misconfigured Routing

Although DNS operates at the Application Layer, attackers frequently abuse DNS during Internet Layer reconnaissance to identify hosts and infrastructure.

---

### Security Considerations

Organizations protect this layer through:

- ACLs
- Firewalls
- Anti-Spoofing Rules
- Network Segmentation
- Secure Routing Policies

---

### Layer 3 — Transport Layer

#### Purpose

The Transport Layer establishes end-to-end communication between applications.

Primary protocols:

- TCP
- UDP

This layer provides connection management, reliability, sequencing, and multiplexing.

---

### Common Attack Surface

Transport Layer attacks frequently target exposed services or connection handling.

Examples include:

- TCP SYN Flood
- Session Hijacking
- Port Scanning
- Banner Grabbing
- UDP Floods
- Service Enumeration

Open TCP or UDP ports often become the initial entry point during external penetration tests.

---

### Security Considerations

Typical defenses include:

- Stateful Firewalls
- Rate Limiting
- SYN Cookies
- Secure Service Configuration
- Network Monitoring

---

### Layer 4 — Application Layer

#### Purpose

The Application Layer contains the protocols directly used by users and applications.

Examples include:

- HTTP
- HTTPS
- DNS
- FTP
- SMTP
- SMB
- LDAP
- Kerberos
- SSH
- RDP

Most enterprise services operate within this layer.

---

### Common Attack Surface

The majority of real-world vulnerabilities exist at the Application Layer.

Examples include:

- SQL Injection
- Cross-Site Scripting (XSS)
- Remote Code Execution
- Authentication Bypass
- API Abuse
- SMB Exploitation
- LDAP Misconfiguration
- Kerberos Abuse
- DNS Misconfiguration
- Credential Leakage

Application Layer weaknesses frequently become the starting point for complete attack chains.

---

### Security Perspective

Each layer contributes to the overall security posture of a network.

A weakness at one layer often enables attacks against higher layers.

For example:

- ARP Spoofing enables traffic interception.
- Captured traffic may expose session tokens.
- Session tokens allow application impersonation.
- Application access may lead to privilege escalation.
- Privilege escalation enables lateral movement.

Real-world compromises rarely involve a single vulnerability. Instead, attackers combine weaknesses across multiple layers to achieve their objectives.

---

### Attacker Mindset

Attackers rarely think in terms of protocol layers.

Instead, they focus on identifying:

- Reachable systems
- Exposed services
- Weak authentication
- Trust relationships
- Insecure protocols
- Misconfigurations
- Opportunities for privilege escalation

The TCP/IP model simply provides a structured framework for understanding where those weaknesses exist.

---

### Layer-to-Attack Mapping

TCP/IP Layer| Typical Attacks
Network Interface| ARP Spoofing, MAC Spoofing, Rogue AP, Sniffing
Internet| IP Spoofing, ICMP Abuse, Host Discovery, Routing Attacks
Transport| Port Scanning, Session Hijacking, SYN Flood, Service Enumeration
Application| SQL Injection, XSS, SSRF, SMB Abuse, LDAP Abuse, Kerberos Abuse, API Exploitation

---

Key Takeaways

- The TCP/IP model reflects how modern networks actually communicate.
- Every layer exposes different attack surfaces.
- Attackers chain weaknesses across multiple layers rather than targeting a single protocol.
- Understanding protocol behavior is essential for both penetration testing and defensive security.
- Strong networking knowledge enables accurate attack path analysis and more realistic security assessments.
