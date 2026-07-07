# Security Identifiers (SID) 

## Overview

A **Security Identifier (SID)** is a unique value assigned by Windows to every security principal in an Active Directory or local Windows environment.

Security principals include:

- Users
- Computers
- Groups
- Service Accounts

From a security perspective:

> Windows never trusts usernames it trusts Security Identifiers (SIDs). Permissions, authentication, and authorization are all based on SIDs rather than object names.

Understanding SIDs is fundamental for:

- Active Directory Security
- Windows Authorization
- Privilege Escalation
- Access Control
- Incident Response
- Forensics

---

# What is a SID?

A SID uniquely identifies a security object.

Examples:

- User Account
- Computer Account
- Security Group
- Domain Controller
- Service Account

Unlike usernames, a SID never changes during the lifetime of an object.

---

# High-Level Architecture

```
User

↓

Username

↓

Security Identifier (SID)

↓

Access Token

↓

Authorization

↓

Resource Access
```

Windows always performs authorization using the SID.

---

# SID Format

A SID looks similar to:

```
S-1-5-21-3623811015-3361044348-30300820-1105
```

General structure:

```
S

↓

Revision

↓

Authority

↓

Domain Identifier

↓

Relative Identifier (RID)
```

---

# SID Components

Example:

```
S-1-5-21-3623811015-3361044348-30300820-1105
```

| Component | Description |
|-----------|-------------|
| S | SID identifier |
| 1 | Revision level |
| 5 | Identifier Authority |
| 21 | Domain identifier type |
| 3623811015-3361044348-30300820 | Domain SID |
| 1105 | Relative Identifier (RID) |

---

# Domain SID

Every Active Directory domain has a unique Domain SID.

Example:

```
S-1-5-21-3623811015-3361044348-30300820
```

All users and groups inside that domain share the same Domain SID.

Only the RID changes.

---

# Relative Identifier (RID)

The RID uniquely identifies an object within a domain.

Example:

```
Domain SID

+

RID

=

Complete SID
```

Example:

```
Domain SID

S-1-5-21-3623811015-3361044348-30300820

RID

500

↓

Administrator SID

S-1-5-21-3623811015-3361044348-30300820-500
```

---

# Well-Known RIDs

Certain RIDs have predefined meanings.

| RID | Object |
|------|---------|
| 500 | Administrator |
| 501 | Guest |
| 502 | KRBTGT |
| 512 | Domain Admins |
| 513 | Domain Users |
| 514 | Domain Guests |
| 515 | Domain Computers |
| 516 | Domain Controllers |
| 518 | Schema Admins |
| 519 | Enterprise Admins |
| 520 | Group Policy Creator Owners |

These values are consistent across Windows domains.

---

# Local SID vs Domain SID

## Local SID

Created on standalone Windows systems.

Example:

```
Computer

↓

Local Administrator

↓

Local SID
```

---

## Domain SID

Created by Active Directory.

Example:

```
Domain

↓

Domain User

↓

Domain SID
```

---

# SID vs Username

| Username | SID |
|----------|-----|
| Can change | Never changes |
| Human-readable | System identifier |
| Used for display | Used for authorization |
| Can be duplicated across domains | Globally unique within its scope |

Windows authorizes access using the SID—not the username.

---

# SID in Access Tokens

After authentication:

```
User

↓

Authentication

↓

Access Token

↓

User SID

+

Group SIDs

↓

Authorization
```

The access token contains:

- User SID
- Group SIDs
- Privileges
- Security information

---

# SID History

Active Directory supports:

```
SID History
```

Purpose:

- Domain migrations
- Resource compatibility

Old SIDs remain attached to the account so legacy permissions continue to function.

Improper management of SID History can introduce security risks.

---

# Security Importance

SIDs are used for:

- Authentication
- Authorization
- ACL evaluation
- Group membership
- Object ownership

Every security decision references SIDs.

---

# Access Control Lists (ACLs)

Permissions are assigned using SIDs.

Example:

```
File

↓

ACL

↓

Allowed SID

↓

Access Granted
```

Even if a username changes, permissions remain because the SID is unchanged.

---

# Object Ownership

Objects record ownership using SIDs.

Examples:

- Files
- Registry Keys
- Active Directory Objects

Ownership remains tied to the SID rather than the account name.

---

# Offensive Perspective

Attackers analyze SIDs to identify:

- Domain SID
- Administrator accounts
- Privileged groups
- Domain Controllers
- Trust relationships

SIDs help map the privilege structure of an Active Directory environment.

---

# Common Attack Targets

| SID | Why It Matters |
|------|----------------|
| Administrator (RID 500) | Built-in administrator |
| KRBTGT (RID 502) | Kerberos ticket generation |
| Domain Admins (RID 512) | Domain-wide administration |
| Enterprise Admins (RID 519) | Forest-wide administration |

These accounts are high-value targets.

---

# Common Enumeration Targets

Attackers enumerate:

- Domain SID
- User SIDs
- Group SIDs
- SID History
- Privileged RIDs
- Domain Controllers

This information helps identify escalation paths.

---

# Defensive Perspective

Administrators should:

- Audit privileged SIDs
- Monitor SID History changes
- Review ACLs regularly
- Protect high-value accounts
- Remove unnecessary privileged memberships
- Monitor authentication events

Proper SID management strengthens authorization security.

---

# SID vs GUID

| SID | GUID |
|------|------|
| Used for security | Used for object identification |
| Used during authorization | Used internally by AD |
| Can include RID | Random unique identifier |
| Appears in ACLs | Appears in directory operations |

Both uniquely identify objects but serve different purposes.

---

# Why SIDs Matter

Understanding SIDs enables security professionals to:

- Analyze Windows authorization
- Understand ACLs
- Investigate privilege escalation
- Secure Active Directory
- Perform forensic analysis
- Assess enterprise permissions

---

# Key Takeaways

- Every security principal receives a unique SID.
- Windows authorizes access using SIDs rather than usernames.
- A SID consists of a Domain SID and a Relative Identifier (RID).
- Well-known RIDs identify built-in accounts and privileged groups.
- Access Tokens contain user and group SIDs.
- SIDs are fundamental to Active Directory's authorization model.

---

# Key Insight

> In Windows, identities are names for humans but SIDs are identities for the operating system. Every permission check, ACL evaluation, and authorization decision ultimately depends on Security Identifiers, making them one of the most fundamental concepts in Active Directory security.
