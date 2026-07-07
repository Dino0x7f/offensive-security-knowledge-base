# Authentication Flow (NTLM & Kerberos) (Windows Internals - Security Perspective)

## Overview

Authentication is the process of verifying the identity of a user before granting access to system resources.

Windows primarily supports two authentication protocols:

- **NTLM (NT LAN Manager)** – Legacy authentication protocol
- **Kerberos** – Modern authentication protocol used in Active Directory environments

From a security perspective:

> Authentication is the foundation of trust in Windows. Every access token, privilege, and authorization decision begins with a successful authentication process.

Understanding how NTLM and Kerberos work is essential for Windows Internals, Active Directory security, privilege escalation, lateral movement, and incident response.

---

# Authentication Architecture

```
User

      │

Credential Provider

      │

Winlogon

      │

LSASS

      │

Authentication Package

      │

NTLM or Kerberos

      │

Access Token

      │

User Session
```

LSASS coordinates the entire authentication process.

---

# Windows Authentication Components

Authentication involves several Windows components:

| Component | Role |
|------------|------|
| Winlogon | Handles interactive logon |
| Credential Provider | Collects credentials |
| LSASS | Coordinates authentication |
| Authentication Package | Verifies credentials |
| SAM | Local account database |
| Active Directory | Domain authentication |
| Security Reference Monitor | Uses Access Token for authorization |

---

# Authentication vs Authorization

Authentication answers:

> Who are you?

Authorization answers:

> What are you allowed to do?

Authentication always occurs before authorization.

---

# Local Authentication

When logging into a standalone Windows computer:

```
User

↓

Winlogon

↓

LSASS

↓

SAM Database

↓

Password Verified

↓

Access Token Created
```

No Domain Controller is involved.

---

# Domain Authentication

In Active Directory:

```
User

↓

LSASS

↓

Kerberos

↓

Domain Controller

↓

Authentication

↓

Access Token
```

Authentication is delegated to the Domain Controller.

---

# NTLM Authentication

## What is NTLM?

NTLM (NT LAN Manager) is Microsoft's legacy challenge-response authentication protocol.

It is commonly used:

- Local authentication
- Legacy systems
- Systems without Kerberos
- Workgroup environments

---

# NTLM Authentication Flow

```
Client

↓

Username

↓

Server

↓

Random Challenge

↓

Client

↓

NTLM Response

↓

Server

↓

LSASS

↓

Authentication Result
```

---

# NTLM Challenge-Response

Simplified process:

```
Client

↓

Request Login

↓

Server Sends Challenge

↓

Client Calculates Response

↓

Server Verifies Response

↓

Authenticated
```

The password itself is never transmitted across the network.

---

# NTLM Components

Authentication involves:

- Username
- NTLM Hash
- Server Challenge
- Client Response

---

# NTLM Characteristics

Advantages:

- Simple
- Backward compatible
- No Domain Controller required

Disadvantages:

- Older protocol
- No mutual authentication
- Susceptible to relay attacks if protections are absent
- Weaker than Kerberos in enterprise environments

---

# NTLM Authentication Example

```
User

↓

Password

↓

NTLM Hash

↓

Challenge

↓

Response

↓

Authenticated
```

---

# Kerberos Authentication

## What is Kerberos?

Kerberos is Microsoft's primary authentication protocol for Active Directory environments.

Instead of using challenge-response authentication, Kerberos uses **tickets**.

---

# Kerberos Goals

Kerberos provides:

- Mutual authentication
- Ticket-based authentication
- Single Sign-On (SSO)
- Strong cryptography
- Reduced password exposure

---

# Kerberos Architecture

```
Client

↓

Domain Controller

↓

Key Distribution Center (KDC)

↓

Ticket

↓

Application Server
```

---

# Key Components

The Key Distribution Center (KDC) consists of:

- Authentication Service (AS)
- Ticket Granting Service (TGS)

---

# Kerberos Authentication Flow

```
User

↓

LSASS

↓

Authentication Service (AS)

↓

Ticket Granting Ticket (TGT)

↓

Ticket Granting Service (TGS)

↓

Service Ticket

↓

Application Server

↓

Access Granted
```

---

# Step 1 — Initial Authentication

```
User

↓

Username + Password

↓

Domain Controller

↓

Authentication Service

↓

Issue TGT
```

---

# Step 2 — Ticket Granting Ticket (TGT)

The TGT proves the user's identity.

```
User

↓

TGT

↓

Stored by LSASS
```

The password is no longer needed after obtaining the TGT.

---

# Step 3 — Request Service Ticket

When accessing a service:

```
Client

↓

TGT

↓

Ticket Granting Service

↓

Service Ticket
```

---

# Step 4 — Access Service

```
Client

↓

Service Ticket

↓

Server

↓

Validate Ticket

↓

Authenticated
```

---

# Kerberos Authentication Diagram

```
User

↓

AS

↓

TGT

↓

TGS

↓

Service Ticket

↓

Target Server
```

---

# Kerberos Tickets

Windows primarily uses:

### Ticket Granting Ticket (TGT)

Used to request additional tickets.

---

### Service Ticket (TGS Ticket)

Used to access specific services.

---

# Kerberos Advantages

Compared to NTLM:

- Faster after initial login
- Mutual authentication
- Strong encryption
- No repeated password validation
- Better scalability
- Supports delegation

---

# NTLM vs Kerberos

| Feature | NTLM | Kerberos |
|----------|------|-----------|
| Password-Based | Challenge-Response | Ticket-Based |
| Mutual Authentication | No | Yes |
| Single Sign-On | Limited | Yes |
| Active Directory | Optional | Primary Protocol |
| Replay Protection | Limited | Strong |
| Modern Enterprise Use | Legacy | Standard |

---

# Authentication Packages

LSASS loads multiple packages:

```
LSASS

├── Kerberos

├── MSV1_0 (NTLM)

├── Negotiate

├── CredSSP
```

The **Negotiate** package automatically selects Kerberos when available and falls back to NTLM if necessary.

---

# Access Token Creation

After successful authentication:

```
Authentication

↓

LSASS

↓

Access Token

↓

Explorer.exe

↓

User Processes
```

Authentication always results in an Access Token.

---

# Authentication Failures

Common reasons include:

- Incorrect password
- Disabled account
- Expired password
- Locked account
- Invalid ticket
- Time synchronization issues (Kerberos)

---

# Security Risks

## NTLM Risks

Attackers may attempt:

- NTLM relay
- Pass-the-Hash
- NTLM hash capture
- Legacy protocol abuse

---

## Kerberos Risks

Attackers may attempt:

- Kerberoasting
- Pass-the-Ticket
- Golden Ticket
- Silver Ticket
- Ticket replay
- Delegation abuse

These attacks target the protocol's implementation or stolen credentials—not the cryptography itself.

---

# Offensive Perspective

During post-exploitation, attackers assess:

- Which authentication protocol is in use
- Domain membership
- Available Kerberos tickets
- Legacy NTLM usage
- Authentication downgrade opportunities

Modern enterprise attacks often focus on abusing authentication artifacts rather than passwords.

---

# Defensive Perspective

Security teams should:

- Prefer Kerberos over NTLM
- Reduce unnecessary NTLM usage
- Enable LDAP signing and SMB signing where appropriate
- Monitor abnormal authentication events
- Detect suspicious ticket usage
- Audit failed authentication attempts
- Protect LSASS and credential material

---

# Pentester Perspective

During an assessment, pentesters evaluate:

- Is NTLM still enabled?
- Are legacy systems forcing NTLM?
- Can authentication be relayed?
- Are Kerberos tickets adequately protected?
- Is time synchronization correctly configured?
- Are privileged accounts authenticating securely?

Authentication analysis often reveals weaknesses that enable lateral movement and privilege escalation.

---

# Relationship with Windows Components

```
Credential Provider

        │

Winlogon

        │

LSASS

        │

Authentication Package

        │

NTLM / Kerberos

        │

Access Token

        │

Security Reference Monitor

        │

Protected Resources
```

Authentication establishes identity, while the Security Reference Monitor enforces permissions.

---

# Authentication Protocol Comparison

| Characteristic | NTLM | Kerberos |
|----------------|------|-----------|
| Introduced | Windows NT | Windows 2000 / Active Directory |
| Authentication Model | Challenge-Response | Ticket-Based |
| Password Sent | No | No |
| Domain Controller Required | No | Yes |
| Mutual Authentication | No | Yes |
| Single Sign-On | Limited | Yes |
| Enterprise Recommendation | Legacy Support | Preferred |

---

# Key Takeaways

- Windows supports both NTLM and Kerberos authentication.
- NTLM uses a challenge-response mechanism and is primarily maintained for compatibility.
- Kerberos is the default authentication protocol in Active Directory environments.
- LSASS coordinates authentication regardless of the protocol used.
- Successful authentication results in the creation of an Access Token.
- Authentication establishes identity, while authorization determines access rights.
- Understanding authentication flows is fundamental for Windows security, Active Directory, and post-exploitation analysis.

---

# Key Insight

> Authentication is the first trust decision Windows makes. Whether through NTLM or Kerberos, the goal is the same: prove identity, create an Access Token, and establish a trusted security context that every subsequent authorization decision depends upon.
