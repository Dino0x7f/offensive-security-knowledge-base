# Authentication Flow 

## Overview

Authentication is the process of verifying the identity of a user, computer, or service before access to resources is granted. In an Active Directory environment, authentication primarily relies on **Kerberos**, with **NTLM** serving as a fallback protocol for legacy or unsupported scenarios.

From a security perspective:

> Authentication is the foundation of Active Directory security. Every authorization decision begins with a successful authentication event.

Understanding the authentication flow is essential for:

- Active Directory Security
- Kerberos Internals
- NTLM Authentication
- Credential Attacks
- Privilege Escalation
- Incident Response
- Enterprise Security

---

# Authentication vs Authorization

These two concepts are often confused.

| Authentication | Authorization |
|----------------|---------------|
| Verifies identity | Determines permissions |
| "Who are you?" | "What can you access?" |
| Happens first | Happens after authentication |

Authentication proves identity.

Authorization determines privileges.

---

# High-Level Authentication Flow

```
User

↓

Client Computer

↓

Domain Controller

↓

Identity Verification

↓

Access Token Created

↓

Resource Access
```

Authentication always occurs before authorization.

---

# Authentication Components

Active Directory authentication involves:

| Component | Purpose |
|-----------|----------|
| User | Requests access |
| Client | Sends authentication request |
| Domain Controller | Verifies identity |
| KDC | Issues Kerberos tickets |
| Resource Server | Grants or denies access |

---

# Logon Process

When a user presses:

```
Ctrl + Alt + Delete
```

Windows performs:

```
Username

+

Password

↓

LSASS

↓

Authentication Package

↓

Domain Controller
```

LSASS coordinates the authentication process.

---

# Kerberos Authentication Flow

If Kerberos is available:

```
User

↓

Client

↓

Authentication Service (AS)

↓

Ticket Granting Ticket (TGT)

↓

Ticket Granting Service (TGS)

↓

Service Ticket

↓

Resource Server

↓

Access Granted
```

Passwords are used only during the initial authentication.

---

# NTLM Authentication Flow

If Kerberos cannot be used:

```
Client

↓

NEGOTIATE

↓

CHALLENGE

↓

AUTHENTICATE

↓

Domain Controller

↓

Authentication Result
```

NTLM uses challenge-response instead of tickets.

---

# Access Token Creation

After successful authentication:

```
User

↓

Domain Controller

↓

Access Token
```

The Access Token contains:

- User SID
- Group SIDs
- Privileges
- User Rights
- Security Information

Windows uses this token for authorization.

---

# Authorization Flow

```
Access Token

↓

ACL Evaluation

↓

Access Decision

↓

Allow / Deny
```

Authentication ends once the Access Token is created.

Authorization begins immediately afterward.

---

# Domain Logon Example

```
User

↓

Windows Logon

↓

LSASS

↓

Kerberos

↓

Domain Controller

↓

Access Token

↓

Desktop
```

The user now has an authenticated session.

---

# Accessing a Network Resource

Example:

```
Authenticated User

↓

File Server

↓

Kerberos Service Ticket

↓

ACL Evaluation

↓

Access Granted
```

The user does not need to re-enter credentials due to Single Sign-On.

---

# Single Sign-On (SSO)

Kerberos supports:

```
Login Once

↓

Receive TGT

↓

Request Multiple Service Tickets

↓

Access Multiple Services
```

This eliminates repeated password prompts.

---

# Authentication Factors

Authentication can involve:

| Factor | Example |
|----------|----------|
| Knowledge | Password |
| Possession | Smart Card |
| Inherence | Fingerprint |
| Multi-Factor | Password + Token |

Modern enterprises increasingly use MFA.

---

# Authentication Protocol Selection

Windows generally follows:

```
Kerberos Available?

↓

Yes

↓

Kerberos

↓

No

↓

NTLM
```

Kerberos is always preferred.

---

# Credential Storage

Credentials may exist in:

- LSASS memory
- Credential Manager
- Kerberos ticket cache
- Smart cards
- TPM-backed storage

These locations are common targets during credential theft attacks.

---

# Authentication Events

Common Windows security events include:

| Event ID | Description |
|-----------|-------------|
| 4624 | Successful Logon |
| 4625 | Failed Logon |
| 4768 | Kerberos TGT Request |
| 4769 | Kerberos Service Ticket |
| 4771 | Kerberos Pre-Authentication Failure |
| 4776 | NTLM Authentication |

These events are important during investigations.

---

# Security Importance

Authentication protects:

- User identities
- Enterprise resources
- Administrative privileges
- Domain Controllers
- Network services

Weak authentication mechanisms expose the entire enterprise.

---

# Offensive Perspective

Attackers target authentication to:

- Steal credentials
- Capture NTLM hashes
- Abuse Kerberos tickets
- Perform Pass-the-Hash
- Perform Pass-the-Ticket
- Escalate privileges
- Move laterally

The authentication process is a primary target in enterprise attacks.

---

# Common Authentication Attacks

| Attack | Description |
|----------|-------------|
| Pass-the-Hash | Reuse NT hash |
| Pass-the-Ticket | Reuse Kerberos tickets |
| Kerberoasting | Crack service account hashes |
| AS-REP Roasting | Target accounts without pre-authentication |
| NTLM Relay | Relay NTLM authentication |
| Credential Dumping | Extract credentials from LSASS |

These attacks exploit authentication mechanisms rather than breaking cryptography.

---

# Defensive Perspective

Administrators should:

- Prefer Kerberos over NTLM
- Enable Multi-Factor Authentication
- Protect LSASS
- Monitor authentication events
- Restrict privileged logons
- Rotate privileged credentials
- Secure Domain Controllers
- Audit failed authentication attempts

Strong authentication significantly reduces attack surface.

---

# Authentication Lifecycle

```
Credential Submission

↓

Identity Verification

↓

Authentication

↓

Access Token Creation

↓

Authorization

↓

Resource Access

↓

Logoff
```

Each stage contributes to the overall security model.

---

# Why Authentication Flow Matters

Understanding authentication flow enables security professionals to:

- Analyze Windows logons
- Investigate credential attacks
- Detect lateral movement
- Understand Kerberos and NTLM
- Secure enterprise authentication
- Perform Active Directory assessments

---

# Key Takeaways

- Authentication verifies identity before access is granted.
- Active Directory primarily uses Kerberos, with NTLM as a fallback.
- LSASS coordinates the authentication process.
- Successful authentication results in an Access Token.
- Authorization evaluates the Access Token against ACLs.
- Most Active Directory attacks target authentication mechanisms or credential material.

---

# Key Insight

> Authentication is the entry point to every Active Directory environment. Whether through Kerberos tickets or NTLM challenge-response, the goal is always the same: establish trust. Understanding how identities are verified, tokens are created, and credentials are protected is fundamental to defending—and assessing—the security of Windows enterprise networks.
