# Kerberos Internals 

## Overview

**Kerberos** is the default authentication protocol used by Active Directory. It provides secure, ticket-based authentication without transmitting user passwords across the network.

From a security perspective:

> Kerberos is the backbone of authentication in modern Windows enterprise environments. Most Active Directory attacks revolve around abusing Kerberos tickets, delegation, or trust relationships rather than breaking the protocol itself.

Understanding Kerberos is essential for:

- Active Directory Security
- Authentication
- Lateral Movement
- Credential Attacks
- Privilege Escalation
- Incident Response
- Red Team Operations

---

# What is Kerberos?

Kerberos is a network authentication protocol that uses:

- Symmetric cryptography
- Trusted third-party authentication
- Time-based tickets

Instead of sending passwords repeatedly, Kerberos issues temporary tickets that prove a user's identity.

---

# Kerberos Goals

Kerberos provides:

- Authentication
- Mutual Authentication
- Single Sign-On (SSO)
- Credential Protection
- Replay Attack Mitigation

---

# Kerberos Components

Kerberos relies on three primary entities.

| Component | Purpose |
|-----------|---------|
| Client | User or computer requesting access |
| Key Distribution Center (KDC) | Issues authentication tickets |
| Service | Resource being accessed |

In Active Directory, the **Domain Controller** hosts the KDC.

---

# High-Level Architecture

```
Client

↓

Key Distribution Center (KDC)

↓

Service Ticket

↓

Application Server

↓

Access Granted
```

---

# Key Components

The KDC contains two logical services:

```
Key Distribution Center

│

├── Authentication Service (AS)

└── Ticket Granting Service (TGS)
```

Each performs a different function.

---

# Authentication Service (AS)

The Authentication Service verifies the user's identity.

Responsibilities:

- Validate credentials
- Generate a Ticket Granting Ticket (TGT)
- Encrypt ticket information

---

# Ticket Granting Service (TGS)

The Ticket Granting Service issues tickets for specific services.

Example:

```
User

↓

TGT

↓

TGS

↓

Service Ticket

↓

SQL Server
```

---

# Ticket Granting Ticket (TGT)

The first ticket issued is the:

```
Ticket Granting Ticket

(TGT)
```

Purpose:

- Proves identity
- Used to request additional tickets
- Eliminates repeated password verification

The TGT is never presented directly to application servers.

---

# Service Ticket (TGS Ticket)

After obtaining a TGT:

```
Client

↓

Requests Service Ticket

↓

KDC

↓

TGS Ticket

↓

Application Server
```

Each service receives its own ticket.

---

# Authentication Flow

```
1. User logs in

↓

2. Client requests TGT

↓

3. KDC verifies credentials

↓

4. KDC returns TGT

↓

5. Client requests Service Ticket

↓

6. KDC returns TGS Ticket

↓

7. Client accesses service

↓

8. Service validates ticket

↓

Access Granted
```

Passwords are never transmitted after the initial authentication.

---

# Single Sign-On (SSO)

One of Kerberos' major advantages:

```
Login Once

↓

Receive TGT

↓

Request Service Tickets

↓

Access Multiple Services
```

Users authenticate once while accessing multiple resources seamlessly.

---

# Mutual Authentication

Kerberos authenticates both parties.

```
Client

⇄

Server
```

Both verify each other's identity before communication begins.

---

# Service Principal Name (SPN)

Every Kerberos-enabled service has an:

```
Service Principal Name

(SPN)
```

Examples:

```
HTTP/webserver

MSSQLSvc/sql01

HOST/server01
```

The SPN uniquely identifies the service.

---

# Session Keys

Kerberos generates temporary session keys.

Used for:

- Encrypting communication
- Authenticating tickets
- Protecting sessions

Session keys are separate from user passwords.

---

# Ticket Lifetime

Tickets are temporary.

Typical values:

- TGT: ~10 hours
- Renewable: Several days

Expiration reduces the impact of stolen tickets.

---

# Time Synchronization

Kerberos depends heavily on accurate system time.

```
Client Time

≈

Domain Controller Time
```

Large clock differences cause authentication failures.

---

# Security Importance

Kerberos protects:

- User identities
- Password confidentiality
- Service authentication
- Single Sign-On
- Enterprise authentication

Nearly every Windows domain authentication depends on Kerberos.

---

# Offensive Perspective

Attackers rarely attack Kerberos directly.

Instead, they abuse:

- Tickets
- Delegation
- SPNs
- Trust relationships
- Service Accounts

Most modern Active Directory attacks involve legitimate Kerberos functionality.

---

# Common Kerberos Attacks

Examples include:

| Attack | Description |
|----------|-------------|
| Kerberoasting | Extract service ticket hashes for offline cracking |
| AS-REP Roasting | Abuse accounts without pre-authentication |
| Pass-the-Ticket | Reuse stolen Kerberos tickets |
| Golden Ticket | Forge TGTs using KRBTGT key |
| Silver Ticket | Forge service tickets |
| Delegation Abuse | Abuse Kerberos delegation |

These attacks exploit configuration or credential weaknesses rather than flaws in the Kerberos protocol.

---

# Common Enumeration Targets

Attackers identify:

- SPNs
- Service Accounts
- Delegation Settings
- KRBTGT Account
- Ticket Lifetimes
- Domain Controllers

This information helps map authentication paths.

---

# KRBTGT Account

The:

```
KRBTGT
```

account is one of the most sensitive accounts in Active Directory.

Purpose:

- Signs Ticket Granting Tickets
- Trusted by every Domain Controller

Compromise of the KRBTGT account can enable forged Kerberos tickets.

---

# Defensive Perspective

Administrators should:

- Protect Domain Controllers
- Secure the KRBTGT account
- Audit SPNs
- Monitor unusual ticket requests
- Restrict delegation
- Enforce strong service account passwords
- Synchronize system time
- Rotate KRBTGT credentials after major incidents

---

# Kerberos vs NTLM

| Kerberos | NTLM |
|-----------|------|
| Ticket-based | Challenge-response |
| Supports SSO | No native SSO |
| Mutual authentication | Client authentication only |
| Faster after initial logon | Authenticates each connection |
| Default in AD | Legacy protocol |

Modern Active Directory environments prefer Kerberos whenever possible.

---

# Why Kerberos Matters

Understanding Kerberos enables security professionals to:

- Analyze Windows authentication
- Understand ticket-based security
- Investigate credential attacks
- Detect lateral movement
- Assess Active Directory security
- Respond to authentication-related incidents

---

# Key Takeaways

- Kerberos is the default authentication protocol in Active Directory.
- It uses tickets instead of repeatedly transmitting credentials.
- The KDC consists of the Authentication Service (AS) and Ticket Granting Service (TGS).
- Authentication begins with a TGT, followed by service-specific tickets.
- SPNs identify Kerberos-enabled services.
- Most Active Directory attacks abuse Kerberos configuration or credentials rather than the protocol itself.

---

# Key Insight

> Kerberos is fundamentally a **trust delegation system**. Instead of repeatedly proving identity with passwords, clients present cryptographically protected tickets issued by a trusted authority. Understanding how tickets are issued, validated, and abused is essential for both defending and attacking modern Active Directory environments.
