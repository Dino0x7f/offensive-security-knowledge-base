# Active Directory Replication (Security Perspective)

## Overview

**Active Directory Replication** is the process by which Domain Controllers synchronize directory information across the enterprise. Every writable Domain Controller maintains a copy of the Active Directory database (NTDS.dit), and replication ensures that all copies remain consistent.

From a security perspective:

> Replication is the mechanism that keeps Active Directory synchronized. Because replication transfers sensitive directory data—including password hashes it is one of the most security-critical operations in an enterprise.

Understanding replication is essential for:

- Active Directory Security
- Domain Controller Internals
- Credential Protection
- DCSync Attacks
- Incident Response
- Enterprise Administration

---

# What is Replication?

Replication is the automatic synchronization of Active Directory objects between Domain Controllers.

Objects include:

- Users
- Computers
- Groups
- Password hashes
- Group Policies
- Organizational Units
- Trusts

---

# High-Level Architecture

```
          Domain Controller 1
                │
        Replication
                │
          Domain Controller 2
                │
        Replication
                │
          Domain Controller 3
```

Each writable Domain Controller maintains a synchronized copy of Active Directory.

---

# Why Replication Exists

Without replication:

```
User Created

↓

Only One Domain Controller Knows
```

With replication:

```
User Created

↓

Replication

↓

All Domain Controllers Updated
```

---

# Multi-Master Replication

Active Directory uses a:

```
Multi-Master Model
```

Meaning:

Every writable Domain Controller can:

- Create objects
- Modify objects
- Delete objects

Changes are later synchronized across the domain.

---

# Replication Data

Replication includes:

- User accounts
- Computer accounts
- Password changes
- Security Identifiers (SIDs)
- ACLs
- GPO information
- Trust relationships
- DNS records (AD-integrated)

---

# Replication Process

```
Object Modified

↓

Update Sequence Number (USN)

↓

Replication Queue

↓

Partner Domain Controller

↓

Database Updated
```

Only changes are replicated.

---

# Replication Partners

Each Domain Controller communicates with one or more:

```
Replication Partners
```

Partners exchange directory updates automatically.

---

# Update Sequence Number (USN)

Every modification receives a:

```
USN
```

Purpose:

- Identify changes
- Prevent duplicate replication
- Track synchronization

Each Domain Controller maintains its own USN values.

---

# Invocation ID

Every Domain Controller has an:

```
Invocation ID
```

It uniquely identifies the Active Directory database instance.

Used to:

- Track replication
- Detect restored databases
- Prevent replication errors

---

# Knowledge Consistency Checker (KCC)

The:

```
Knowledge Consistency Checker

(KCC)
```

Automatically builds the replication topology.

Responsibilities:

- Create replication connections
- Optimize replication paths
- Maintain fault tolerance

Administrators rarely configure replication manually.

---

# Replication Topology

```
DC1

↔

DC2

↔

DC3

↔

DC4
```

The topology minimizes bandwidth while ensuring redundancy.

---

# Intra-Site Replication

Occurs within the same Active Directory Site.

Characteristics:

- Frequent
- Fast
- RPC-based
- High bandwidth assumption

---

# Inter-Site Replication

Occurs between different Sites.

Characteristics:

- Compressed
- Scheduled
- Bandwidth-aware
- Configurable intervals

---

# Active Directory Sites

Sites represent physical network locations.

Example:

```
New York

↓

Site A

↓

London

↓

Site B
```

Replication behavior depends on Site configuration.

---

# Replication Transport

Common transports:

- RPC (default)
- SMTP (limited legacy scenarios)

RPC is used in almost all modern environments.

---

# Password Replication

Password changes receive special handling.

When a password changes:

```
User Changes Password

↓

PDC Emulator Updated

↓

Replication Begins

↓

Other Domain Controllers Updated
```

Password replication is prioritized.

---

# SYSVOL Replication

In addition to NTDS.dit:

Domain Controllers replicate:

```
SYSVOL
```

Contains:

- Group Policies
- Login Scripts
- Administrative Templates

Modern environments use:

```
DFS Replication (DFSR)
```

---

# Security Importance

Replication distributes:

- Credentials
- Password hashes
- Administrative objects
- Trust information
- Security policies

Compromising replication compromises enterprise security.

---

# Replication Permissions

Certain accounts possess replication rights.

Important permissions include:

- Replicating Directory Changes
- Replicating Directory Changes All
- Replicating Directory Changes In Filtered Set

These permissions should be tightly controlled.

---

# Offensive Perspective

Attackers target replication because it provides direct access to Active Directory data.

Objectives include:

- Retrieve password hashes
- Dump directory information
- Enumerate users
- Extract secrets
- Compromise the domain

---

# DCSync Attack

One of the most important Active Directory attacks.

```
Attacker

↓

Requests Replication

↓

Domain Controller

↓

Returns Password Data
```

Instead of stealing **NTDS.dit**, attackers impersonate a Domain Controller and request replication.

If successful, they can retrieve:

- NTLM hashes
- Kerberos keys
- KRBTGT account secrets

---

# Common Attack Targets

| Target | Objective |
|----------|-----------|
| NTDS Replication | Credential extraction |
| DCSync Rights | Password dumping |
| KRBTGT | Golden Ticket attacks |
| Replication Permissions | Domain compromise |
| Domain Controllers | Full directory access |

---

# Common Misconfigurations

Examples include:

- Excessive replication permissions
- Non-admin accounts with DCSync rights
- Unmonitored replication requests
- Weak Domain Controller security
- Poor delegation practices

These significantly increase enterprise risk.

---

# Defensive Perspective

Administrators should:

- Restrict replication permissions
- Audit DCSync-capable accounts
- Monitor replication activity
- Secure Domain Controllers
- Review delegated rights
- Monitor unusual replication requests
- Protect privileged accounts

Replication should only occur between legitimate Domain Controllers.

---

# Replication vs Authentication

| Replication | Authentication |
|--------------|---------------|
| Synchronizes directory data | Verifies identity |
| Domain Controller ↔ Domain Controller | User ↔ Domain Controller |
| Transfers directory changes | Issues tickets or validates credentials |
| Background process | User-initiated process |

---

# Why Replication Matters

Understanding replication enables security professionals to:

- Analyze Active Directory synchronization
- Detect DCSync attacks
- Investigate credential theft
- Understand Domain Controller communication
- Secure enterprise identity infrastructure
- Perform Active Directory assessments

---

# Key Takeaways

- Active Directory uses multi-master replication to synchronize Domain Controllers.
- Replication distributes users, groups, credentials, policies, and directory changes.
- The Knowledge Consistency Checker (KCC) automatically builds the replication topology.
- Replication occurs differently within Sites and between Sites.
- Replication permissions are highly sensitive because they allow access to credential data.
- DCSync abuses legitimate replication functionality to retrieve password hashes.

---

# Key Insight

> Replication is the heartbeat of Active Directory. Every Domain Controller depends on it to maintain a consistent view of the enterprise. Because replication transfers the directory's most sensitive information—including password hashes controlling replication rights is just as important as protecting Domain Controllers themselves.
