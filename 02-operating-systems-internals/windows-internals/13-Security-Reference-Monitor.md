# Security Reference Monitor (SRM) (Windows Internals - Security Perspective)

## Overview

The **Security Reference Monitor (SRM)** is a kernel-mode component responsible for enforcing Windows security policies.

Every attempt to access a protected object such as a file, process, registry key, or service is ultimately evaluated by the SRM.

From a security perspective:

> The Security Reference Monitor is Windows' central authorization engine. It decides whether an operation is allowed or denied based on security policies and access rights.

Unlike authentication, which verifies *who* a user is, the SRM determines *what that user is allowed to do*.

---

# Why SRM Exists

Modern operating systems require a trusted mechanism that enforces access control consistently.

Without SRM:

- Any process could access any file
- Malware could freely modify system resources
- Privilege boundaries would not exist

The SRM ensures that every protected object follows Windows security rules.

---

# SRM Architecture

```
Application

      │

Windows API

      │

Object Manager

      │

Security Reference Monitor

      │

Access Decision

      │

Kernel Object
```

Before an application accesses a protected object, SRM evaluates the request.

---

# Responsibilities of SRM

The Security Reference Monitor is responsible for:

- Access control
- Permission enforcement
- Privilege validation
- Security auditing
- Object protection
- Security descriptor evaluation

---

# Authentication vs Authorization

Authentication:

```
Who are you?
```

Authorization:

```
What are you allowed to do?
```

Authentication is performed by **LSASS**.

Authorization is enforced by the **Security Reference Monitor**.

---

# Protected Objects

SRM protects nearly every securable object in Windows.

Examples include:

- Files
- Directories
- Processes
- Threads
- Registry Keys
- Services
- Named Pipes
- Events
- Mutexes
- Shared Memory
- Tokens

---

# Access Request Flow

When a process attempts to access an object:

```
Application

↓

Open File

↓

Object Manager

↓

SRM

↓

Check Permissions

↓

Allow / Deny
```

Every protected operation passes through this workflow.

---

# Security Tokens

SRM relies on **Access Tokens** to determine permissions.

Each running process possesses a token containing:

- User SID
- Group SIDs
- Privileges
- Integrity Level
- Default DACL

Example:

```
Process

↓

Access Token

↓

SRM
```

The token represents the security identity of the process.

---

# Security Identifiers (SID)

Windows identifies users using **Security Identifiers (SIDs)** instead of usernames.

Example:

```
S-1-5-21-...
```

SRM compares SIDs against object permissions.

---

# Security Descriptors

Every protected object has a **Security Descriptor**.

It contains:

- Owner
- Group
- DACL
- SACL

Example:

```
File

↓

Security Descriptor

↓

Permissions
```

---

# Discretionary Access Control List (DACL)

The DACL defines **who is allowed or denied access**.

Example:

```
User A → Read

User B → Full Control

User C → Denied
```

SRM evaluates the DACL before granting access.

---

# System Access Control List (SACL)

Unlike DACLs, SACLs do not grant permissions.

Instead, they define:

- What actions should be audited
- Which security events generate logs

Example:

```
Delete File

↓

Generate Audit Event
```

---

# Access Check Process

SRM performs the following steps:

1. Obtain process token
2. Read object security descriptor
3. Compare requested access
4. Evaluate privileges
5. Return allow or deny

Workflow:

```
Access Token

↓

Security Descriptor

↓

Access Check

↓

Decision
```

---

# Access Rights

Examples of access rights include:

Files:

- Read
- Write
- Execute
- Delete

Processes:

- Terminate
- Suspend
- Read Memory
- Write Memory

Registry:

- Query
- Set Value
- Create Subkey

Each object type has its own permissions.

---

# Privileges

Privileges differ from permissions.

Examples:

- SeDebugPrivilege
- SeBackupPrivilege
- SeRestorePrivilege
- SeShutdownPrivilege
- SeTakeOwnershipPrivilege

Privileges are stored in the process token.

---

## Security Relevance

Some privileges allow bypassing normal permission checks.

Example:

```
SeDebugPrivilege

↓

Open Any Process

↓

Memory Access
```

Attackers frequently attempt to acquire these privileges.

---

# Integrity Levels

Modern Windows includes **Mandatory Integrity Control (MIC).**

Integrity Levels:

- Low
- Medium
- High
- System

Example:

```
Medium Process

↓

High Process

↓

Access Denied
```

Integrity levels add another layer of protection beyond ACLs.

---

# Mandatory Integrity Control

SRM also evaluates Integrity Levels.

Even if the DACL allows access:

```
Medium Process

↓

High Object

↓

Blocked
```

This prevents lower-integrity processes from modifying higher-integrity objects.

---

# User Account Control (UAC)

UAC works alongside SRM.

When elevation occurs:

```
Standard Token

↓

Elevation

↓

Administrator Token
```

SRM evaluates whichever token is currently active.

---

# Auditing

If auditing is enabled:

```
Access Attempt

↓

SRM

↓

Audit Event

↓

Security Log
```

The SRM sends audit records to the Windows Event Log.

---

# Object Manager Integration

SRM closely collaborates with the Object Manager.

```
Application

↓

Object Manager

↓

SRM

↓

Kernel Object
```

The Object Manager resolves the object, while SRM determines access rights.

---

# Common Objects Evaluated

Examples include:

| Object | Security Checked |
|---------|------------------|
| Files | DACL |
| Registry | DACL |
| Processes | Access Rights |
| Services | Service ACL |
| Named Pipes | DACL |
| Mutexes | Security Descriptor |

---

# Security Policies

SRM enforces policies including:

- File permissions
- Service permissions
- Registry permissions
- Process security
- Object ownership
- Privilege assignment

---

# Common Offensive Techniques

Attackers frequently attempt to bypass or abuse SRM indirectly.

Examples:

### Token Theft

Steal another process token.

```
Victim Token

↓

Attacker Process

↓

SRM Trusts Token
```

---

### Token Impersonation

Use another user's security context.

---

### Privilege Escalation

Acquire powerful privileges such as:

```
SeDebugPrivilege
```

---

### ACL Misconfiguration

Weak DACLs may allow:

- File overwrite
- Service modification
- Registry modification

without exploiting software vulnerabilities.

---

### Handle Duplication

If an attacker duplicates a privileged handle, SRM has already approved the original access.

---

# Defensive Perspective

Security products monitor:

- Privilege changes
- Token duplication
- ACL modifications
- Unauthorized access attempts
- Security descriptor changes
- Audit log generation

Unexpected SRM-related activity often indicates privilege escalation attempts.

---

# Pentester Perspective

During a Windows assessment, pentesters analyze:

- Weak DACLs
- Service permissions
- Registry permissions
- Dangerous privileges
- Token impersonation opportunities
- Object ownership
- Integrity levels

Many local privilege escalation vulnerabilities stem from improperly configured security descriptors rather than software flaws.

---

# Relationship with Other Windows Components

```
Application

      │

Windows API

      │

Object Manager

      │

Security Reference Monitor

      │

Security Descriptor

      │

Access Token

      │

Kernel Object
```

The SRM acts as the enforcement point between user requests and protected system resources.

---

# Common Attack Techniques Related to SRM

| Technique | Component Abused |
|-----------|------------------|
| Token Theft | Access Token |
| Token Impersonation | Security Token |
| Privilege Escalation | Privileges |
| Weak ACL Abuse | DACL |
| Service Hijacking | Service Permissions |
| Registry Hijacking | Registry ACL |
| Process Injection | SeDebugPrivilege |

---

# Key Takeaways

- The Security Reference Monitor is Windows' kernel-mode authorization engine.
- It determines whether a process can access protected system objects.
- SRM evaluates Access Tokens, Security Descriptors, DACLs, SACLs, privileges, and Integrity Levels.
- Authentication is handled by LSASS, while authorization is enforced by SRM.
- Weak permissions, excessive privileges, and ACL misconfigurations are common attack vectors.
- Understanding SRM is fundamental for Windows privilege escalation, malware analysis, digital forensics, and defensive security.

---

# Key Insight

> The Security Reference Monitor is the guardian of Windows authorization. Every successful privilege escalation, unauthorized file access, or protected object modification ultimately depends on convincing or bypassing the SRM to grant access.
