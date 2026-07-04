# Common Network Protocols (Security Perspective)

### Introduction

Network protocols define the rules that enable communication between systems. Every request sent across a network—whether browsing a website, accessing a file share, authenticating to Active Directory, or transferring data—relies on one or more communication protocols.

From an offensive security perspective, protocols are not merely communication mechanisms. They define the available attack surface, expose trust relationships, and often become the primary targets during reconnaissance, exploitation, and post-exploitation activities.

Understanding protocol behavior is essential for identifying vulnerabilities, recognizing insecure configurations, and analyzing realistic attack paths.

---

## Why Protocols Matter

Every penetration test involves interacting with network protocols.

Attackers analyze protocols to:

- Identify exposed services.
- Determine authentication mechanisms.
- Enumerate software and operating systems.
- Detect insecure configurations.
- Discover trust relationships.
- Locate sensitive assets.
- Build attack chains.

A protocol itself is not a vulnerability. However, weak implementations, insecure configurations, or outdated software frequently introduce exploitable conditions.

---

## HTTP / HTTPS

#### Purpose

HTTP and HTTPS provide communication between web clients and web servers.

These protocols support:

- Websites
- Web Applications
- REST APIs
- Administrative Interfaces
- Cloud Services

Because most organizations expose web applications to the Internet, HTTP and HTTPS are among the most frequently assessed protocols during penetration tests.

---

## Common Security Risks

Typical attack vectors include:

- SQL Injection
- Cross-Site Scripting (XSS)
- Server-Side Request Forgery (SSRF)
- Authentication Bypass
- Session Hijacking
- Insecure Cookies
- Weak TLS Configuration
- Information Disclosure

Application-layer vulnerabilities often become the initial entry point for complete attack chains.

---

## DNS

#### Purpose

The Domain Name System (DNS) translates domain names into IP addresses, enabling users and applications to locate network resources.

DNS is fundamental to nearly every modern network environment.

---

## Common Security Risks

Attackers frequently abuse DNS for:

- Zone Transfer Enumeration
- DNS Cache Poisoning
- DNS Spoofing
- Subdomain Enumeration
- Data Exfiltration via DNS
- Internal Infrastructure Discovery

Even when securely configured, DNS often reveals valuable information during reconnaissance.

---

## SMB

#### Purpose

Server Message Block (SMB) enables file sharing, printer sharing, and resource access in Windows environments.

SMB is heavily integrated with Active Directory infrastructures.

---

## Common Security Risks

Typical attack vectors include:

- SMB Relay
- Pass-the-Hash
- NTLM Relay
- Anonymous Shares
- Weak Share Permissions
- Lateral Movement
- Remote Service Execution
- Legacy SMB Vulnerabilities

SMB is one of the most significant protocols encountered during internal penetration tests.

---

## FTP

#### Purpose

File Transfer Protocol (FTP) enables file exchange between systems.

Although largely replaced by secure alternatives, FTP remains present in many legacy environments.

---

## Common Security Risks

Common weaknesses include:

- Plaintext Credentials
- Anonymous Authentication
- Sensitive File Exposure
- Weak Access Controls
- Information Disclosure

Because FTP transmits data without encryption, intercepted traffic may expose usernames, passwords, and transferred files.

---

## SSH

#### Purpose

Secure Shell (SSH) provides encrypted remote administration and secure file transfer for Unix-like systems.

SSH is widely used by system administrators and cloud environments.

---

## Common Security Risks

Typical attack vectors include:

- Password Brute Force
- Credential Reuse
- Weak SSH Keys
- Stolen Private Keys
- Misconfigured Access Controls
- Outdated SSH Implementations

Compromised SSH access often grants direct administrative control over target systems.

---

## RDP

#### Purpose

Remote Desktop Protocol (RDP) enables graphical remote administration of Windows systems.

It is commonly used for server management and enterprise administration.

---

## Common Security Risks

Common risks include:

- Credential Brute Force
- Password Spraying
- Credential Stuffing
- Remote Code Execution (historical vulnerabilities)
- Exposed Administrative Services
- Weak Multi-Factor Authentication

Internet-exposed RDP services remain a frequent target for attackers.

---

## SMTP, IMAP and POP3

#### Purpose

These protocols support electronic mail services.

- SMTP is responsible for sending email.
- IMAP allows synchronized mailbox access.
- POP3 retrieves email from a server.

Email infrastructure is often a high-value target because it contains sensitive communications and supports account recovery.

---

## Common Security Risks

Attackers commonly exploit:

- Email Spoofing
- Open Mail Relays
- Credential Theft
- Weak Authentication
- Misconfigured Mail Servers
- Phishing Infrastructure
- Sensitive Information Disclosure

Compromising email services frequently enables further attacks against users and enterprise systems.

---

## Attacker Perspective

Attackers analyze network protocols to answer critical questions during reconnaissance:

- Which services are exposed?
- Which authentication methods are used?
- Which protocols contain known weaknesses?
- Which services are externally accessible?
- Which protocols enable lateral movement?
- Which trust relationships exist between systems?

Rather than targeting protocols indiscriminately, attackers prioritize those that provide the greatest opportunity for privilege escalation or access expansion.

---

## Defensive Perspective

Security teams focus on securing protocol implementations through:

- Service Hardening
- Strong Authentication
- Encryption
- Least Privilege
- Timely Security Updates
- Network Segmentation
- Continuous Monitoring
- Protocol-Specific Security Policies

Reducing unnecessary protocol exposure significantly decreases the organization's attack surface.

---

## Protocol-to-Attack Mapping

Protocol| Typical Attacks
HTTP / HTTPS| SQL Injection, XSS, SSRF, 
Authentication Bypass

DNS| Zone Transfers, Cache Poisoning, DNS Spoofing, Subdomain Enumeration

SMB| Pass-the-Hash, SMB Relay, NTLM 
Relay, Lateral Movement

FTP| Anonymous Access, Plaintext Credential Capture, File Disclosure

SSH| Brute Force, Key Theft, Credential Reuse

RDP| Password Spraying, Brute Force, Administrative Compromise

SMTP / IMAP / POP3| Email Spoofing, Credential Theft, Mail Server Misconfiguration

---

## Key Takeaways

- Network protocols define how systems communicate and interact.
- Every protocol introduces unique attack surfaces and security considerations.
- Most successful attacks exploit insecure protocol implementations rather than the protocols themselves.
- Understanding protocol behavior enables accurate reconnaissance, vulnerability assessment, and attack path analysis.
- Strong protocol security depends on secure configuration, proper authentication, encryption, and continuous monitoring rather than protocol choice alone.
