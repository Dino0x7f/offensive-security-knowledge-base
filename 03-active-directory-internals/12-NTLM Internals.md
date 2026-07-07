# NTLM Internals 

## Overview

**NTLM (NT LAN Manager)** is Microsoft's legacy authentication protocol used when Kerberos cannot be used. Although Kerberos is the default authentication protocol in Active Directory, NTLM remains widely supported for backward compatibility.

From a security perspective:

> NTLM is significantly less secure than Kerberos. Many modern Active Directory attacks exploit weaknesses in NTLM authentication, relay mechanisms, or credential handling.

Understanding NTLM is essential for:

- Active Directory Security
- Windows Authentication
- Credential Attacks
- Lateral Movement
- Incident Response
- Enterprise Security

---

# What is NTLM?

NTLM is a challenge-response authentication protocol.

Instead of sending the user's password over the network:

- The server sends a challenge.
- The client computes a cryptographic response.
- The server verifies the response.

---

# Why NTLM Exists

NTLM was introduced before Kerberos.

It is still used when:

- Kerberos is unavailable.
- The server is not domain joined.
- IP addresses are used instead of hostnames.
- Cross-forest trust is unavailable.
- Legacy systems require NTLM.

---

# High-Level Architecture

```
Client

↓

Server

↓

Challenge

↓

NTLM Response

↓

Authentication
```

Unlike Kerberos, NTLM does not use tickets.

---

# Authentication Components

Three entities participate:

| Component | Purpose |
|-----------|----------|
| Client | User requesting authentication |
| Server | Resource being accessed |
| Domain Controller | Validates credentials (for domain accounts) |

---

# Authentication Flow

```
Client

↓

Authentication Request

↓

Server

↓

Challenge

↓

Client

↓

Challenge Response

↓

Server

↓

Domain Controller

↓

Authentication Result
```

The password itself is never transmitted.

---

# NTLM Challenge-Response

Authentication occurs in three messages.

```
NEGOTIATE

↓

CHALLENGE

↓

AUTHENTICATE
```

---

## Step 1 — Negotiate

The client sends:

- Supported NTLM version
- Security capabilities

```
Client

↓

NEGOTIATE
```

---

## Step 2 — Challenge

The server replies with:

- Random challenge (nonce)
- Supported options

```
Server

↓

CHALLENGE
```

---

## Step 3 — Authenticate

The client:

- Computes the response using the NT hash
- Sends username
- Sends challenge response

```
Client

↓

AUTHENTICATE
```

The server forwards the information to the Domain Controller if necessary.

---

# NT Hash

NTLM authentication relies on the:

```
NT Hash
```

Characteristics:

- Derived from the password
- Stored in Active Directory
- Used during authentication

The original password is never sent.

---

# NTLM Versions

Microsoft introduced multiple versions.

| Version | Status |
|----------|--------|
| LM | Obsolete |
| NTLM | Legacy |
| NTLMv2 | Current recommended version |

LM should never be enabled.

---

# Local Authentication

If logging into a standalone computer:

```
Client

↓

Local SAM Database

↓

Authentication
```

No Domain Controller is involved.

---

# Domain Authentication

For domain users:

```
Client

↓

Server

↓

Domain Controller

↓

Authentication
```

The Domain Controller validates the NTLM response.

---

# Session Security

After authentication:

- Session key generated
- Optional message signing
- Optional message encryption

These features improve integrity but are not universally enforced.

---

# Security Importance

NTLM provides:

- Legacy authentication
- Compatibility
- Password protection through challenge-response

However, it lacks many protections found in Kerberos.

---

# Limitations

NTLM:

- No Single Sign-On
- No mutual authentication
- Uses challenge-response
- Requires repeated authentication
- More susceptible to relay attacks

Because of these limitations, Kerberos is preferred whenever possible.

---

# NTLM vs Kerberos

| NTLM | Kerberos |
|------|-----------|
| Challenge-response | Ticket-based |
| No mutual authentication | Mutual authentication |
| Legacy protocol | Modern protocol |
| Re-authenticates frequently | Single Sign-On |
| More vulnerable to relay attacks | Stronger authentication model |

---

# Pass-the-Hash

One of the most well-known NTLM attacks is:

```
Pass-the-Hash
```

Instead of stealing passwords:

Attackers steal:

```
NT Hash

↓

Authenticate

↓

Access Granted
```

The password itself is never needed.

---

# NTLM Relay

Another common attack:

```
Victim

↓

Authenticates

↓

Attacker Relays Authentication

↓

Target Server
```

The attacker forwards authentication without knowing the password.

This attack exploits trust between systems rather than breaking NTLM cryptography.

---

# Common NTLM Attacks

Examples include:

| Attack | Description |
|----------|-------------|
| Pass-the-Hash | Reuse NT hash |
| NTLM Relay | Forward authentication |
| Credential Dumping | Extract NT hashes |
| SMB Relay | Relay SMB authentication |
| Authentication Coercion | Force NTLM authentication |

Most attacks abuse protocol behavior rather than cryptographic weaknesses.

---

# Common Enumeration Targets

Attackers identify:

- NTLM enabled services
- SMB servers
- Domain Controllers
- Local administrator reuse
- NTLM policies
- Signing configuration

---

# Defensive Perspective

Administrators should:

- Prefer Kerberos
- Disable LM authentication
- Require NTLMv2
- Enable SMB Signing
- Restrict NTLM usage
- Disable unnecessary NTLM authentication
- Monitor NTLM traffic
- Use Credential Guard where applicable

Reducing NTLM usage significantly decreases attack surface.

---

# Enterprise Recommendations

Microsoft recommends:

- Prefer Kerberos whenever possible.
- Audit NTLM usage.
- Disable NTLM where supported.
- Require SMB signing.
- Implement Extended Protection for Authentication (EPA).
- Protect credentials with Credential Guard.

---

# Why NTLM Matters

Understanding NTLM enables security professionals to:

- Analyze Windows authentication
- Investigate credential attacks
- Detect relay attacks
- Understand Pass-the-Hash
- Secure legacy environments
- Assess authentication risks

---

# Key Takeaways

- NTLM is Microsoft's legacy authentication protocol.
- It uses challenge-response instead of ticket-based authentication.
- Authentication relies on the NT hash rather than transmitting passwords.
- NTLM lacks mutual authentication and Single Sign-On.
- Common attacks include Pass-the-Hash and NTLM Relay.
- Modern enterprise environments should minimize NTLM usage in favor of Kerberos.

---

# Key Insight

> NTLM was designed for compatibility, not modern threat models. While it protects passwords from being transmitted directly, its reliance on reusable credential material and challenge-response authentication makes it a common target for credential theft and relay attacks. Understanding NTLM is essential because many enterprise attacks succeed by exploiting where NTLM is still enabled rather than by defeating Kerberos.
