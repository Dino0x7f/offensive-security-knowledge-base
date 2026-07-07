# Active Directory Database (NTDS.dit) 

## Overview

The **Active Directory Database**, stored in the **NTDS.dit** file, is the central repository of all directory information within an Active Directory domain.

It contains every object, attribute, permission, and security relationship required for the operation of Active Directory.

From a security perspective:

> NTDS.dit is one of the most valuable assets in a Windows enterprise. Compromising it provides attackers with identities, password hashes, trust relationships, and the information required to compromise an entire domain.

Understanding NTDS.dit is fundamental for:

- Active Directory Security
- Credential Attacks
- Incident Response
- Malware Analysis
- Privilege Escalation
- Digital Forensics

---

# What is NTDS.dit?

NTDS.dit is an **Extensible Storage Engine (ESE)** database used by Active Directory Domain Services (AD DS).

It stores:

- Users
- Computers
- Groups
- Organizational Units
- Password hashes
- Security Identifiers (SIDs)
- Group memberships
- Access Control Lists (ACLs)
- Trust relationships
- Group Policy information

Every writable Domain Controller maintains a local copy.

---

# Database Location

By default:

```
C:\Windows\NTDS\NTDS.dit
```

The database is protected by the operating system and normally accessible only by highly privileged accounts.

---

# High-Level Architecture

```
Domain Controller

│

├── NTDS.dit
│      │
│      ├── Users
│      ├── Computers
│      ├── Groups
│      ├── ACLs
│      ├── SIDs
│      ├── Password Data
│      └── Trust Information
│
└── Active Directory Services
```

NTDS.dit serves as the backend database for Active Directory.

---

# Database Engine

Active Directory uses Microsoft's:

```
Extensible Storage Engine (ESE)

or

Jet Blue
```

Features include:

- Indexed storage
- Transaction logging
- Recovery support
- High-performance searches

---

# What Does NTDS.dit Store?

## Identity Objects

Examples:

- Users
- Computers
- Service Accounts
- Groups

---

## Security Information

Including:

- Security Identifiers (SIDs)
- Group memberships
- Access Control Lists
- Security descriptors

---

## Authentication Data

Examples:

- NT password hashes
- Kerberos-related information
- Password metadata

Passwords are **not stored in plaintext**.

---

## Directory Objects

The database stores nearly every AD object.

Examples:

```
User

Computer

OU

Printer

Shared Folder

Contact

Group Policy Container
```

---

# Password Storage

NTDS.dit contains password hashes used during authentication.

Examples include:

- NT Hash
- Kerberos-related credential data

The actual passwords are never stored directly.

---

# Relationship with SYSTEM Hive

Password information is protected using secrets derived from the:

```
SYSTEM Registry Hive
```

Both NTDS.dit and the SYSTEM hive are required to recover password hashes offline.

---

# Replication

Every writable Domain Controller stores its own copy of NTDS.dit.

```
DC1

⇄

DC2

⇄

DC3
```

Changes are replicated automatically using Active Directory replication.

---

# Transaction Logs

Active Directory uses transaction logs to maintain consistency.

Examples:

```
edb.log

edbxxxxx.log
```

These logs ensure database recovery after crashes or unexpected shutdowns.

---

# Security Importance

NTDS.dit contains nearly everything required to manage the domain.

Examples:

- Domain users
- Administrative groups
- Password hashes
- Trust relationships
- Group memberships
- Enterprise configuration

Because of this, NTDS.dit is considered a **Tier-0 asset**.

---

# Offensive Perspective

Attackers target NTDS.dit because it contains:

- Credential material
- Password hashes
- Administrative accounts
- Service accounts
- Trust information

Access to the database can significantly accelerate domain compromise.

---

# Common Attack Objectives

Attackers attempt to:

- Obtain NTDS.dit
- Extract password hashes
- Identify privileged accounts
- Enumerate trust relationships
- Discover service accounts
- Build attack graphs

---

# Common Attack Paths

| Target | Objective |
|---------|-----------|
| NTDS.dit | Credential extraction |
| SYSTEM Hive | Decrypt password data |
| Domain Controller | Database access |
| Replication Services | Directory synchronization abuse |
| Backup Files | Offline database extraction |

---

# Defensive Perspective

Administrators should:

- Protect Domain Controllers
- Restrict administrative access
- Encrypt backups
- Monitor access to NTDS files
- Secure registry hives
- Audit privileged activity
- Limit interactive logons on Domain Controllers

Protecting NTDS.dit is essential to protecting the domain.

---

# NTDS.dit vs Registry

| NTDS.dit | Windows Registry |
|-----------|------------------|
| Active Directory database | Operating system configuration |
| Stores directory objects | Stores local settings |
| Domain-wide information | Machine-specific information |
| Replicated between DCs | Local to each machine |

---

# NTDS.dit vs SAM

| NTDS.dit | SAM |
|-----------|-----|
| Domain accounts | Local accounts |
| Stored on Domain Controllers | Stored on all Windows systems |
| Enterprise authentication | Local authentication |
| Supports Kerberos & LDAP | Local logon database |

---

# Why NTDS.dit Matters

Understanding NTDS.dit helps security professionals:

- Understand Active Directory internals
- Investigate credential attacks
- Perform forensic analysis
- Secure Domain Controllers
- Analyze authentication data
- Assess enterprise security posture

---

# Key Takeaways

- NTDS.dit is the Active Directory database stored on Domain Controllers.
- It uses Microsoft's Extensible Storage Engine (ESE).
- It stores users, groups, computers, SIDs, ACLs, and password hashes.
- Passwords are stored as cryptographic hashes, not plaintext.
- Every writable Domain Controller maintains a replicated copy.
- NTDS.dit is one of the highest-value assets in an Active Directory environment.

---

# Key Insight

> NTDS.dit is the heart of Active Directory. It contains the identities, relationships, and credential data that define an enterprise's trust model. While attackers view it as the ultimate source of credentials, defenders must treat it as one of the most critical assets to protect, monitor, and recover.
