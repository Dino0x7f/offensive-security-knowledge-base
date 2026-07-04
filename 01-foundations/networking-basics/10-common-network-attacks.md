# Common Network Attacks (Security Perspective)

## Introduction

Network attacks target the communication mechanisms that enable systems to exchange information. Rather than exploiting a single vulnerability, attackers often abuse weaknesses in network protocols, trust relationships, and insecure configurations to gain visibility, intercept traffic, expand access, or disrupt services.

Understanding these attacks is essential for penetration testers because they form the foundation of many real-world attack chains. Techniques such as Man-in-the-Middle attacks, ARP spoofing, DNS manipulation, and session hijacking are frequently combined with application or authentication weaknesses to achieve broader objectives.

This chapter provides a high-level overview of the most common network attacks from a security perspective. Detailed attack methodologies are covered later in the Attack Chains section.

---

### Why Network Attacks Matter

Every enterprise depends on network communication.

Attackers target network protocols to:

- Discover systems.
- Intercept sensitive communications.
- Capture authentication data.
- Bypass trust relationships.
- Expand access across the network.
- Disrupt business operations.

Unlike application attacks, network attacks often affect multiple systems simultaneously because they target the communication infrastructure itself.

---

## Man-in-the-Middle (MITM)

### Overview

A Man-in-the-Middle (MITM) attack occurs when an attacker positions themselves between two communicating systems, allowing traffic to be observed or modified without either party realizing it.

MITM attacks often rely on weaknesses in lower-layer protocols rather than software vulnerabilities.

### Common Objectives

- Credential interception
- Session theft
- Data manipulation
- Traffic inspection
- Authentication relay

---

## ARP Spoofing

### Overview

ARP Spoofing exploits the lack of authentication in the Address Resolution Protocol (ARP).

By sending forged ARP replies, an attacker associates their own MAC address with another system's IP address, causing traffic to be redirected.

Typical Impact

- Man-in-the-Middle attacks
- Credential interception
- Traffic manipulation
- Internal reconnaissance
- Authentication relay opportunities

ARP spoofing is one of the most common techniques used during internal penetration tests.

---

## DNS Spoofing

### Overview

DNS Spoofing manipulates DNS responses so that victims resolve legitimate domain names to attacker-controlled IP addresses.

Victims believe they are communicating with trusted systems while interacting with malicious infrastructure.

Typical Impact

- Phishing
- Credential theft
- Malware delivery
- Traffic redirection
- Infrastructure impersonation

---

## Packet Sniffing

### Overview

Packet sniffing involves capturing network traffic for analysis.

While packet capture is a legitimate administrative activity, attackers use it to collect valuable information transmitted across the network.

Information Commonly Collected

- Cleartext credentials
- Session cookies
- Internal IP addresses
- Authentication traffic
- Application requests
- Network topology information

The value of packet sniffing depends largely on whether sensitive communications are properly encrypted.

---

## Session Hijacking

### Overview

Many applications maintain authenticated user sessions after successful login.

If attackers obtain valid session identifiers, they may impersonate legitimate users without knowing their passwords.

Potential Impact

- Unauthorized application access
- Account impersonation
- Privilege abuse
- Business logic exploitation

Session hijacking often follows credential interception or application-layer weaknesses.

---

## Replay Attacks

### Overview

Replay attacks occur when previously captured legitimate communications are retransmitted in an attempt to gain unauthorized access or repeat an authenticated action.

Rather than breaking encryption, attackers abuse protocols that fail to validate message freshness.

Potential Impact

- Authentication bypass
- Unauthorized transactions
- Protocol abuse
- Identity impersonation

Modern authentication protocols reduce replay risks through timestamps, nonces, and cryptographic verification.

---

## Denial of Service (DoS)

### Overview

Denial of Service attacks attempt to make systems or services unavailable by exhausting available resources.

When attacks originate from many systems simultaneously, they become Distributed Denial of Service (DDoS) attacks.

Common Targets

- Web servers
- DNS infrastructure
- VPN gateways
- Firewalls
- Authentication services

Unlike most network attacks, DoS attacks focus on availability rather than unauthorized access.

---

## Port Scanning

### Overview

Port scanning is a reconnaissance technique used to identify accessible services on target systems.

Although scanning is not inherently malicious, it frequently represents the first phase of an attack.

Objectives

- Discover exposed services
- Identify running applications
- Detect software versions
- Prioritize targets
- Build attack paths

The information gathered during scanning guides every subsequent phase of a penetration test.

---

## Attack Chain Perspective

Network attacks rarely occur in isolation.

A typical attack progression may resemble:

Reconnaissance
        │
        ▼
Port Scanning
        │
        ▼
Service Enumeration
        │
        ▼
ARP Spoofing / DNS Manipulation
        │
        ▼
Traffic Interception
        │
        ▼
Credential Capture
        │
        ▼
Session Hijacking
        │
        ▼
Lateral Movement
        │
        ▼
Privilege Escalation

Each technique contributes to a larger attack chain rather than serving as the final objective.

---

## Defensive Perspective

Organizations reduce the risk of network attacks through layered security controls, including:

- Network Segmentation
- Secure Switch Configuration
- Dynamic ARP Inspection
- DNS Security
- Strong Authentication
- Encrypted Communications
- Intrusion Detection Systems
- Continuous Traffic Monitoring

No single control prevents every network attack; effective defense relies on multiple complementary security measures.

---

## Common Misconceptions

Several misconceptions frequently arise:

- Network attacks only affect large organizations.
- Encryption prevents every form of network attack.
- Internal networks are trusted by default.
- Port scanning is equivalent to exploitation.

In reality, attackers often combine multiple network techniques with application and authentication weaknesses to achieve their objectives.

---

## Key Takeaways

- Network attacks exploit weaknesses in communication protocols, trust relationships, and network configurations.
- Most successful compromises involve multiple techniques rather than a single attack.
- Reconnaissance, traffic interception, credential theft, and lateral movement are closely connected.
- Understanding network attacks is essential before studying enterprise attack chains, Active Directory attacks, and post-exploitation techniques.
- Modern defensive strategies rely on segmentation, secure protocol configuration, monitoring, and layered security controls to reduce attack opportunities.
