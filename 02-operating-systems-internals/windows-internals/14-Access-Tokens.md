# Access Tokens (Windows Internals - Security Perspective)

## Overview

An **Access Token** is a kernel object that represents the security identity and privileges of a user or process in Windows.

Whenever a user logs on, Windows creates an Access Token that defines **who the user is** and **what the user is allowed to do**.

Every process started by that user inherits a copy of this token.

From a security perspective:

> Access Tokens are the foundation of Windows authorization. Attackers rarely "become Administrator" they steal or manipulate tokens that already possess administrative privileges.

Understanding Access Tokens is essential for privilege escalation, impersonation, process security, malware analysis, and Windows authentication.

---

# Why Access Tokens Exist

Windows separates:

- Authentication
- Authorization

Authentication answers:

> Who are you?

Authorization answers:

> What are you allowed to do?

After authentication succeeds, Windows creates an Access Token to represent that identity.

---

# High-Level Architecture

```
User Login

      │

Authentication (LSASS)

      │

Access Token Created

      │

Attached to Process

      │

Used During Access Checks
```

Every security decision depends on the Access Token.

---

# Token Lifecycle

```
User Login

↓

LSASS

↓

Access Token

↓

User Shell (Explorer.exe)

↓

Child Processes

↓

Inherited Token
```

Most user applications inherit the same Access Token.

---

# Where Tokens Are Stored

Access Tokens are Kernel Objects.

They exist in kernel memory and are managed by the Object Manager.

Applications never access them directly.

---

# Token Structure

A typical Access Token contains:

- User SID
- Group SIDs
- Privileges
- Integrity Level
- Default DACL
- Token Type
- Logon Session
- Owner SID

```
Access Token

├── User SID

├── Groups

├── Privileges

├── Integrity Level

├── Owner

└── Default DACL
```

---

# User SID

Every token contains the SID of the authenticated user.

Example:

```
S-1-5-21-...
```

The SID uniquely identifies the user throughout the system.

---

# Group SIDs

A user may belong to multiple groups.

Example:

```
Administrators

Remote Desktop Users

Backup Operators

Users
```

Group membership influences authorization decisions.

---

# Privileges

Privileges grant special operating system capabilities.

Examples:

- SeDebugPrivilege
- SeBackupPrivilege
- SeRestorePrivilege
- SeShutdownPrivilege
- SeTakeOwnershipPrivilege
- SeLoadDriverPrivilege

Privileges are separate from file permissions.

---

# Integrity Level

Modern Windows assigns an Integrity Level to every token.

Levels include:

- Low
- Medium
- High
- System

Example:

```
Browser

↓

Low Integrity

↓

Cannot Modify

↓

High Integrity Process
```

Integrity Levels implement Mandatory Integrity Control (MIC).

---

# Default DACL

When a process creates new objects, Windows uses the Default DACL stored inside the Access Token.

Example:

```
Process

↓

Create File

↓

Default DACL Applied
```

---

# Primary Token

Every process has exactly one Primary Token.

```
Process

↓

Primary Token

↓

Security Context
```

This token defines the process's identity.

---

# Impersonation Token

Threads may temporarily use another token.

```
Thread

↓

Impersonation Token

↓

Access Resource

↓

Revert
```

Used heavily by:

- SMB
- RPC
- IIS
- Services

---

# Token Types

Windows supports two token types:

### Primary Token

Used by processes.

---

### Impersonation Token

Used by threads.

Allows temporary identity switching.

---

# Access Check Process

Whenever access is requested:

```
Process

↓

Access Token

↓

Security Reference Monitor

↓

Security Descriptor

↓

Allow / Deny
```

The SRM compares the token against the object's DACL.

---

# Process Inheritance

When a process creates another process:

```
Parent Process

↓

CreateProcess()

↓

Child Process

↓

Inherited Token
```

Most applications inherit their parent's security context.

---

# UAC and Access Tokens

User Account Control (UAC) creates two tokens:

```
Administrator

↓

Full Token

↓

Filtered Token
```

Normal applications use the filtered token.

Administrative tasks require elevation.

---

# Split Token

Administrators actually possess:

```
Administrator Account

↓

Full Token

+

Limited Token
```

Elevation switches from the limited token to the full token.

---

# Token Duplication

Windows allows privileged processes to duplicate tokens.

API example:

```
DuplicateToken()

DuplicateTokenEx()
```

---

# Token Impersonation

A service may impersonate a client:

```
Client

↓

Connect

↓

Server

↓

Impersonate Client

↓

Access Resources
```

After completing the task:

```
RevertToSelf()
```

---

# Token Stealing

One of the most common privilege escalation techniques.

Example:

```
SYSTEM Process

↓

SYSTEM Token

↓

Duplicate

↓

Assign to Attacker

↓

SYSTEM Shell
```

The attacker never breaks authentication.

They simply reuse an existing trusted token.

---

# Token Theft

Typical workflow:

```
Locate SYSTEM Process

↓

Open Process

↓

Duplicate Token

↓

Spawn New Process

↓

SYSTEM Access
```

---

# Privilege Escalation

Many Windows privilege escalation attacks involve:

- Token theft
- Token duplication
- Token impersonation
- Privileged service abuse

Rather than exploiting authentication itself.

---

# Common Windows APIs

Developers interact with tokens using:

```
OpenProcessToken()

OpenThreadToken()

GetTokenInformation()

AdjustTokenPrivileges()

DuplicateToken()

ImpersonateLoggedOnUser()

CreateProcessAsUser()
```

---

# Access Token and LSASS

During logon:

```
User Login

↓

LSASS

↓

Authentication

↓

Token Creation

↓

Returned to Winlogon
```

LSASS creates the initial token after successful authentication.

---

# Access Token and SRM

The Security Reference Monitor relies entirely on the Access Token.

```
Process

↓

Access Token

↓

SRM

↓

Access Decision
```

No token means no authorization.

---

# Offensive Perspective

Attackers target Access Tokens because they represent trust.

Common objectives include:

- Stealing SYSTEM tokens
- Impersonating privileged users
- Escalating privileges
- Moving laterally
- Bypassing access restrictions

Token manipulation is far easier than breaking Windows authentication.

---

# Defensive Perspective

Security tools monitor:

- Token duplication
- Privilege adjustments
- Token impersonation
- Unexpected SYSTEM processes
- Abuse of SeDebugPrivilege
- New privileged logon sessions

Suspicious token activity often indicates compromise.

---

# Pentester Perspective

During an assessment, pentesters look for:

- Processes running as SYSTEM
- Token impersonation opportunities
- Enabled high-risk privileges
- Misconfigured services
- Weak privilege assignments
- Vulnerable named pipes
- SeImpersonatePrivilege abuse

Modern Windows privilege escalation frequently revolves around Access Tokens rather than kernel exploits.

---

# Relationship with Other Windows Components

```
Authentication

        │

LSASS

        │

Access Token

        │

Process

        │

Security Reference Monitor

        │

Security Descriptor

        │

Access Decision
```

The Access Token bridges authentication and authorization.

---

# Common Offensive Techniques

| Technique | Token Usage |
|-----------|-------------|
| Token Theft | Duplicate privileged token |
| Token Impersonation | Act as another user |
| Pass-the-Token | Reuse an existing token |
| Potato Exploits | Abuse SeImpersonatePrivilege |
| Service Abuse | Obtain SYSTEM token |
| UAC Bypass | Execute with elevated token |
| Privilege Adjustment | Enable disabled privileges |

---

# Key Takeaways

- An Access Token represents the security identity of a process or thread.
- Tokens are created after successful authentication.
- Every process has a Primary Token.
- Threads may temporarily use Impersonation Tokens.
- Tokens contain SIDs, privileges, integrity levels, and security information.
- The Security Reference Monitor relies on tokens during every access check.
- Token theft and impersonation are common privilege escalation techniques.
- Protecting privileged tokens is critical for Windows security.

---

# Key Insight

> In Windows, identity is not tied to a usernamei t is tied to the Access Token. If an attacker acquires a privileged token, Windows treats them as the legitimate owner of that identity, making Access Tokens one of the most valuable targets in modern post-exploitation.
