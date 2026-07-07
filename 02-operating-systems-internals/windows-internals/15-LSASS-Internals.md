# LSASS Internals (Windows Internals - Security Perspective)

## Overview

**LSASS (Local Security Authority Subsystem Service)** is one of the most critical security processes in Windows.

Its primary role is to enforce the local security policy, authenticate users, create access tokens, and manage authentication credentials.

From a security perspective:

> LSASS is the heart of Windows authentication. Compromising LSASS often means compromising the entire system.

For this reason, LSASS is one of the most valuable targets during post-exploitation and one of the most protected processes in modern Windows.

---

# What is LSASS?

LSASS is implemented by:

```
lsass.exe
```

It starts during the Windows boot process and remains running until shutdown.

Location:

```
C:\Windows\System32\lsass.exe
```

It runs as:

```
NT AUTHORITY\SYSTEM
```

---

# Why LSASS Exists

Windows needs a trusted component to:

- Authenticate users
- Manage credentials
- Enforce security policy
- Generate access tokens
- Handle password changes
- Coordinate authentication packages

LSASS performs all of these functions.

---

# High-Level Architecture

```
User Login

      │

Winlogon

      │

Credential Provider

      │

LSASS

      │

Authentication Package

      │

User Verified

      │

Access Token Created
```

---

# Responsibilities

LSASS is responsible for:

- User authentication
- Local security policy
- Access Token creation
- Password management
- Kerberos authentication
- NTLM authentication
- Security auditing
- Credential storage (temporary)

---

# Authentication Flow

```
User

↓

Username + Password

↓

Winlogon

↓

LSASS

↓

Authentication Package

↓

Verify Credentials

↓

Create Access Token

↓

Launch User Session
```

---

# Authentication Packages

LSASS loads several authentication packages.

Examples:

- Kerberos
- MSV1_0 (NTLM)
- Negotiate
- WDigest (legacy)
- CredSSP

Each package supports different authentication mechanisms.

---

# Kerberos

In Active Directory environments:

```
User

↓

Kerberos

↓

Domain Controller

↓

Ticket Issued

↓

LSASS

↓

Access Granted
```

LSASS manages Kerberos tickets for logged-in users.

---

# NTLM

Older environments use NTLM.

```
Client

↓

Challenge

↓

Response

↓

LSASS

↓

Authentication
```

NTLM is still widely encountered for compatibility.

---

# Access Token Creation

After successful authentication:

```
LSASS

↓

Create Token

↓

Attach Token

↓

Explorer.exe

↓

User Session
```

Every user session begins with a token created by LSASS.

---

# Security Policy

LSASS enforces:

- Password policy
- Account lockout policy
- Audit policy
- Logon rights
- Privilege assignments

These settings are defined in Local Security Policy or Group Policy.

---

# Password Changes

When users change passwords:

```
User

↓

LSASS

↓

SAM / Active Directory

↓

Password Updated
```

---

# Security Auditing

LSASS generates security events such as:

- Successful logon
- Failed logon
- Privilege use
- Account lockout
- Password changes

These events appear in:

```
Windows Security Event Log
```

---

# LSASS Memory

LSASS temporarily stores authentication material needed for active sessions.

Depending on configuration, this may include:

- NTLM-related authentication data
- Kerberos tickets
- Logon session information
- Access Tokens
- Security identifiers (SIDs)

Modern Windows includes protections to reduce credential exposure, but LSASS memory remains a high-value target.

---

# Why Attackers Target LSASS

Attackers target LSASS because it contains information related to authenticated users.

Compromising LSASS may allow attackers to:

- Reuse authenticated sessions
- Perform lateral movement
- Escalate privileges
- Impersonate users

---

# LSASS Process

```
System

↓

lsass.exe

↓

Authentication

↓

Security Policy

↓

Credential Management
```

It normally runs continuously.

Unexpected termination causes Windows to become unstable or reboot.

---

# LSASS and Winlogon

```
User Login

↓

Winlogon

↓

LSASS

↓

Authentication

↓

User Session
```

Winlogon manages the login interface.

LSASS verifies credentials.

---

# LSASS and SAM

For local accounts:

```
LSASS

↓

SAM Database

↓

Verify Password

↓

Authenticated
```

---

# LSASS and Active Directory

For domain users:

```
LSASS

↓

Kerberos

↓

Domain Controller

↓

Ticket

↓

Authenticated
```

---

# LSASS and Access Tokens

```
Authentication

↓

LSASS

↓

Access Token

↓

Process Creation
```

Every interactive logon depends on LSASS.

---

# LSASS and SRM

```
LSASS

↓

Create Token

↓

SRM

↓

Use Token

↓

Access Decision
```

LSASS creates the identity.

The Security Reference Monitor enforces permissions.

---

# Protected Process Light (PPL)

Modern Windows protects LSASS using:

```
Protected Process Light (PPL)
```

Benefits:

- Prevents unauthorized memory access
- Blocks many debugging attempts
- Makes credential theft more difficult

PPL significantly raises the bar for attackers but does not eliminate all attack paths.

---

# LSASS Protection

Additional protections include:

- Credential Guard
- Virtualization-Based Security (VBS)
- Windows Defender protections
- Attack Surface Reduction (ASR) rules

These features reduce the risk of credential theft.

---

# Offensive Perspective

During post-exploitation, attackers often seek to:

- Enumerate authenticated sessions
- Obtain Kerberos tickets
- Abuse delegated credentials
- Escalate privileges through authenticated identities

Direct interaction with LSASS is a common objective because it manages the system's authentication state.

---

# Defensive Perspective

Security teams monitor for:

- Unexpected access to `lsass.exe`
- Attempts to obtain debug privileges
- Suspicious process handle requests
- Memory access targeting LSASS
- Creation of memory dumps involving LSASS

Such behavior is frequently associated with credential theft techniques.

---

# Pentester Perspective

During an assessment, pentesters evaluate whether the environment adequately protects LSASS.

Common checks include:

- Is LSASS running as a Protected Process Light (PPL)?
- Is Credential Guard enabled?
- Are unnecessary high-risk privileges assigned?
- Are endpoint security controls monitoring LSASS access?
- Can privileged processes interact with LSASS unexpectedly?

The objective is to assess the resilience of the authentication subsystem rather than to attack LSASS itself.

---

# Relationship with Other Windows Components

```
Winlogon

      │

Credential Provider

      │

LSASS

      │

Authentication Package

      │

Access Token

      │

Security Reference Monitor

      │

Protected Resources
```

LSASS serves as the bridge between authentication and authorization.

---

# Common Security Technologies Related to LSASS

| Technology | Purpose |
|------------|---------|
| Kerberos | Domain authentication |
| NTLM | Legacy authentication |
| SAM | Local account verification |
| Access Tokens | User identity representation |
| Credential Guard | Credential isolation |
| PPL | Process protection |
| SRM | Authorization enforcement |
| Event Logging | Security auditing |

---

# Key Takeaways

- LSASS (`lsass.exe`) is the core Windows authentication process.
- It verifies user credentials using authentication packages such as Kerberos and NTLM.
- After successful authentication, LSASS creates the Access Token used throughout the session.
- It enforces local security policy, password policies, and audit policies.
- LSASS works closely with Winlogon, the SAM database, Active Directory, and the Security Reference Monitor.
- Modern Windows protects LSASS using technologies such as Protected Process Light (PPL) and Credential Guard.
- Because LSASS manages authentication and security context, it remains one of the most critical components in Windows security.

---

# Key Insight

> LSASS is not simply another Windows service it is the trusted authority that transforms credentials into authenticated identities. Every logon, privilege assignment, and access token begins with LSASS, making it the central pillar of Windows authentication.
