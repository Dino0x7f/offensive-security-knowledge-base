# ARP (Security Perspective)
## Introduction

The Address Resolution Protocol (ARP) is a Layer 2 protocol used to map IPv4 addresses to physical MAC addresses within a local network.

Whenever one device wants to communicate with another host on the same subnet, it must first determine the destination's MAC address. ARP provides this mapping by translating an IP address into its corresponding hardware address.

From an offensive security perspective, ARP is one of the weakest protocols in modern networking because it provides no authentication or integrity verification.

This lack of security makes ARP one of the primary targets for internal network attacks.

---

# Why ARP Matters

ARP operates transparently in almost every local network.

Attackers abuse ARP to:

- Redirect network traffic
- Perform Man-in-the-Middle (MITM) attacks
- Capture credentials
- Intercept sensitive communications
- Manipulate network flows
- Launch NTLM Relay attacks
- Facilitate lateral movement

Because ARP is trusted by default, successful attacks often require no software vulnerabilities.

---

# How ARP Works

When a device needs to communicate with another system on the same subnet, it performs the following process:

1. The sender knows the destination IP address.
2. The sender checks its local ARP cache.
3. If no mapping exists, it broadcasts an ARP Request.
4. Every device receives the broadcast.
5. The owner of the requested IP replies with an ARP Reply containing its MAC address.
6. The sender stores the mapping in its ARP cache.
7. Communication begins using Ethernet frames.

This process occurs automatically without user interaction.

---

# ARP Messages

ARP uses two primary message types.

## ARP Request

Broadcast message asking:

> "Who has IP address 192.168.1.20?"

Every host on the subnet receives the request.

---

## ARP Reply

The target host responds:

> "192.168.1.20 is at AA:BB:CC:DD:EE:FF"

The sender stores this information inside its ARP cache.

---

# ARP Cache

Operating systems maintain a temporary ARP cache containing recently resolved IP-to-MAC mappings.

Example:

```text
192.168.1.1     00:50:56:AA:10:01
192.168.1.20    00:0C:29:44:32:10
192.168.1.35    3C:52:82:71:92:10
```

Caching reduces unnecessary broadcasts and improves communication efficiency.

However, because entries are updated dynamically, attackers can manipulate them.

---

# Security Weaknesses

ARP was designed for trusted local networks.

It does not provide:

- Authentication
- Integrity verification
- Encryption
- Sender validation

As a result, any device can advertise false IP-to-MAC mappings.

Hosts generally trust the newest ARP information they receive.

This design flaw forms the basis of ARP Spoofing attacks.

---

# Common ARP-Based Attacks

## ARP Spoofing

An attacker sends forged ARP replies that associate the attacker's MAC address with another system's IP address.

Victims update their ARP caches and begin sending traffic to the attacker instead of the legitimate host.

This attack enables traffic interception without exploiting software vulnerabilities.

---

## Man-in-the-Middle (MITM)

After poisoning multiple ARP caches, the attacker positions themselves between two communicating systems.

The attacker can:

- Observe traffic
- Modify packets
- Inject malicious data
- Steal credentials
- Hijack sessions

MITM attacks are among the most common consequences of ARP Spoofing.

---

## Credential Interception

Protocols that transmit credentials without strong protection may expose:

- HTTP Authentication
- FTP Credentials
- Telnet Sessions
- SMB Authentication
- NTLM Challenge-Response Traffic

Captured authentication traffic may later support relay attacks or credential cracking.

---

## Denial of Service

Incorrect ARP entries may redirect traffic toward nonexistent devices.

Victims lose connectivity because packets never reach their intended destination.

---

# Attacker Perspective

After obtaining access to an internal network, attackers frequently evaluate whether ARP manipulation is possible.

Typical objectives include:

- Redirect network traffic
- Capture authentication exchanges
- Enumerate active hosts
- Identify gateway communications
- Intercept sensitive sessions
- Launch NTLM Relay attacks
- Expand control within the network

Because ARP attacks require local network access, they are particularly valuable during internal penetration tests.

---

# Defensive Perspective

Organizations reduce ARP-related risks through multiple defensive measures.

Common protections include:

- Dynamic ARP Inspection (DAI)
- DHCP Snooping
- Port Security
- Static ARP Entries (where appropriate)
- Network Segmentation
- Secure Switch Configuration
- Encrypted Application Protocols
- Continuous Network Monitoring

Although ARP cannot be secured directly, its abuse can often be detected or mitigated through proper network design.

---

# Common Misconceptions

Several misconceptions frequently appear:

- ARP works across the Internet.
- ARP provides authentication.
- Switches automatically prevent ARP attacks.
- Encryption prevents ARP Spoofing.

In reality, ARP operates only within local broadcast domains and offers no mechanism for verifying the legitimacy of ARP messages.

---

# Real-World Example

Consider a workstation communicating with its default gateway.

```text
Workstation
      │
      │
Gateway
```

After ARP Spoofing:

```text
Workstation
      │
      ▼
Attacker
      │
      ▼
Gateway
```

The workstation believes the attacker's MAC address belongs to the gateway, causing all traffic to pass through the attacker's system.

This enables packet inspection, manipulation, credential interception, and session hijacking.

---

# Key Takeaways

- ARP resolves IPv4 addresses into MAC addresses within a local network.
- ARP provides no authentication or integrity verification.
- Attackers exploit this weakness through ARP Spoofing and Man-in-the-Middle attacks.
- Successful ARP attacks frequently enable credential interception, NTLM Relay, and lateral movement.
- Modern enterprise networks rely on switch security features, segmentation, and encrypted protocols to mitigate ARP-related threats.
