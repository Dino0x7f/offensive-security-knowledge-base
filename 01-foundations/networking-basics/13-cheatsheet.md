# Networking Cheatsheet (Security Perspective)

## Core Networking Concepts

| Concept | Description |
|---------|-------------|
| IP Address | Unique logical identifier assigned to a network device. |
| MAC Address | Physical hardware address used within a local network. |
| Port | Logical endpoint through which a service communicates. |
| Protocol | Set of rules governing network communication. |
| Subnet | Logical division of a network into smaller segments. |
| Gateway | Device responsible for forwarding traffic between networks. |
| DNS | Resolves domain names into IP addresses. |
| ARP | Maps IPv4 addresses to MAC addresses within a LAN. |

---

# Common Ports

| Port | Service | Security Relevance |
|------|---------|--------------------|
| 20/21 | FTP | Plaintext credentials |
| 22 | SSH | Remote administration, brute-force attacks |
| 23 | Telnet | Insecure remote access |
| 25 | SMTP | Email infrastructure |
| 53 | DNS | Enumeration, spoofing, poisoning |
| 67/68 | DHCP | Rogue DHCP attacks |
| 69 | TFTP | Unauthenticated file transfers |
| 80 | HTTP | Web application attacks |
| 110 | POP3 | Email retrieval |
| 123 | NTP | Time synchronization abuse |
| 135 | RPC | Windows service discovery |
| 137-139 | NetBIOS | Legacy Windows networking |
| 143 | IMAP | Email retrieval |
| 161 | SNMP | Information disclosure |
| 389 | LDAP | Directory enumeration |
| 443 | HTTPS | Secure web applications |
| 445 | SMB | File sharing, lateral movement |
| 636 | LDAPS | Secure LDAP |
| 1433 | MSSQL | Database services |
| 3306 | MySQL | Database services |
| 3389 | RDP | Remote desktop access |
| 5432 | PostgreSQL | Database services |
| 5985 | WinRM | Windows Remote Management |
| 5986 | WinRM (HTTPS) | Secure remote management |

---

# Common Protocol Risks

| Protocol | Typical Risks |
|----------|---------------|
| HTTP | XSS, SQLi, SSRF, Session Hijacking |
| HTTPS | Weak TLS, Misconfigurations |
| DNS | Spoofing, Cache Poisoning, Zone Transfers |
| SMB | NTLM Relay, Pass-the-Hash, Lateral Movement |
| LDAP | Information Disclosure |
| Kerberos | Kerberoasting, Ticket Abuse |
| FTP | Plaintext Credentials |
| SSH | Brute Force, Weak Keys |
| RDP | Password Spraying, Credential Theft |
| ARP | ARP Spoofing, MITM |
| DHCP | Rogue DHCP Servers |
| SNMP | Information Disclosure |

---

# Common Network Attacks

| Attack | Goal |
|--------|------|
| Port Scanning | Discover exposed services |
| Service Enumeration | Collect system information |
| Packet Sniffing | Capture network traffic |
| Man-in-the-Middle (MITM) | Intercept communications |
| ARP Spoofing | Redirect LAN traffic |
| DNS Spoofing | Redirect victims |
| Session Hijacking | Steal authenticated sessions |
| Replay Attack | Reuse captured authentication |
| Password Spraying | Test weak passwords |
| DoS / DDoS | Disrupt service availability |

---

# Common Network Weaknesses

- Flat network architecture
- Poor network segmentation
- Open administrative services
- Weak authentication
- Excessive trust relationships
- Legacy protocols
- Misconfigured firewalls
- Outdated operating systems
- Unencrypted communications
- Default credentials

---

# Common Security Controls

- Firewalls
- IDS / IPS
- VLAN Segmentation
- Access Control Lists (ACLs)
- VPN
- TLS Encryption
- Dynamic ARP Inspection (DAI)
- DHCP Snooping
- Multi-Factor Authentication (MFA)
- Network Access Control (NAC)

---

# Typical Penetration Testing Workflow

```text
Reconnaissance
      │
      ▼
Host Discovery
      │
      ▼
Port Scanning
      │
      ▼
Service Enumeration
      │
      ▼
Vulnerability Assessment
      │
      ▼
Initial Access
      │
      ▼
Privilege Escalation
      │
      ▼
Lateral Movement
      │
      ▼
Post-Exploitation
```

---

# Quick Pentester Checklist

## Before Scanning

- Identify target scope
- Verify network reachability
- Determine internal or external assessment
- Identify trust boundaries

---

## During Enumeration

- Identify live hosts
- Enumerate open ports
- Fingerprint services
- Identify operating systems
- Discover authentication services

---

## During Exploitation

- Validate vulnerabilities
- Avoid unnecessary noise
- Document findings
- Collect evidence

---

## During Post-Exploitation

- Enumerate users
- Search for credentials
- Identify trust relationships
- Assess lateral movement opportunities
- Document impact

---

# Pentester Mindset

Always ask:

- What is reachable?
- What is exposed?
- What is trusted?
- What can be abused?
- What leads to the next objective?

---

# Key Takeaways

- Every exposed service increases the attack surface.
- Enumeration is more valuable than blind exploitation.
- Network segmentation significantly limits attacker movement.
- Understanding protocols is more important than memorizing tools.
- Methodology always comes before tool usage.
