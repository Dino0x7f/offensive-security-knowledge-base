# Domain Controller (Security Perspective)

## Overview

A **Domain Controller (DC)** is the most critical server in an Active Directory environment. It hosts Active Directory Domain Services (AD DS) and is responsible for authenticating users, authorizing access, enforcing security policies, and maintaining the Active Directory database.

From a security perspective:

> The Domain Controller is the central authority of the domain. Compromising a Domain Controller typically results in full control over the entire Active Directory environment.

Understanding Domain Controllers is fundamental for:

- Active Directory Security
- Enterprise Penetration Testing
- Red Team Operations
- Incident Response
- Privilege Escalation Analysis
- Lateral Movement

---

# What is a Domain Controller?

A Domain Controller is a Windows Server that stores a copy of the Active Directory database and provides authentication and directory services to domain-joined devices.

Every authentication request inside the domain is ultimately handled by a Domain Controller.

---

# High-Level Architecture

```
                Domain Controller

        ┌────────────────────────────┐
        │ Active Directory Database  │
        │        (NTDS.dit)          │
        ├────────────────────────────┤
        │ Kerberos                   │
        │ LDAP                       │
        │ DNS                        │
        │ Group Policy               │
        │ Replication                │
        └────────────────────────────┘
```

The Domain Controller acts as the trusted authority for identities and resources.

---

# Core Responsibilities

A Domain Controller is responsible for:

- User authentication
- Computer authentication
- Authorization
- Directory services
- Password management
- Group Policy distribution
- Directory replication
- DNS integration
- Trust management

---

# Active Directory Database

Every Domain Controller stores:

```
NTDS.dit
```

The database contains:

- Users
- Computers
- Groups
- Password hashes
- Security Identifiers (SIDs)
- Organizational Units
- Trust relationships
- Access Control Lists (ACLs)

All writable Domain Controllers maintain a synchronized copy through replication.

---

# Authentication Services

The Domain Controller authenticates identities using:

- Kerberos (default)
- NTLM (legacy compatibility)

Authentication process:

```
User

↓

Domain Controller

↓

Identity Verification

↓

Access Token Creation

↓

Resource Access
```

---

# Authorization Services

After successful authentication, the Domain Controller determines what resources a user can access.

Authorization is based on:

- Group membership
- Security descriptors
- Access Control Lists (ACLs)
- User privileges
- Access Tokens

---

# LDAP Services

LDAP (Lightweight Directory Access Protocol) provides directory access.

LDAP allows systems to:

- Query users
- Search groups
- Retrieve objects
- Modify directory information (with proper permissions)

Many administrative tools rely on LDAP.

---

# DNS Integration

Active Directory depends on DNS.

DNS allows clients to locate:

- Domain Controllers
- Kerberos services
- LDAP services
- Global Catalog servers

Without DNS, domain authentication cannot function correctly.

---

# Group Policy Distribution

Domain Controllers distribute:

- Password policies
- Security policies
- Software deployment
- Login scripts
- Administrative templates

Group Policies are replicated between Domain Controllers.

---

# Replication

Multiple Domain Controllers improve:

- Availability
- Fault tolerance
- Load distribution

Replication ensures all Domain Controllers share consistent directory data.

```
DC1

⇄

DC2

⇄

DC3
```

Changes made on one Domain Controller are replicated to others.

---

# Global Catalog

Some Domain Controllers host the **Global Catalog (GC)**.

The Global Catalog contains a searchable subset of objects from every domain within the forest.

Used for:

- Enterprise-wide searches
- User logon
- Universal Group membership

---

# FSMO Roles

Certain operations are handled by specialized Domain Controllers known as **Flexible Single Master Operations (FSMO)** role holders.

The five FSMO roles are:

| Role | Purpose |
|------|---------|
| Schema Master | Controls schema updates |
| Domain Naming Master | Manages domain creation/removal |
| RID Master | Allocates Relative IDs |
| PDC Emulator | Time sync, password changes, legacy support |
| Infrastructure Master | Cross-domain reference updates |

These roles prevent conflicts during critical operations.

---

# Security Importance

A Domain Controller stores or controls:

- User identities
- Password hashes
- Kerberos tickets
- Trust relationships
- Administrative privileges
- Group Policies
- Domain secrets

Because of this, it is the highest-value target in an Active Directory environment.

---

# Common Services Running

Typical services include:

| Service | Purpose |
|---------|---------|
| Active Directory Domain Services | Directory management |
| Kerberos | Authentication |
| LDAP | Directory queries |
| DNS | Name resolution |
| Netlogon | Domain logon support |
| DFS Replication | SYSVOL replication |

---

# Offensive Perspective

Attackers target Domain Controllers because they provide access to:

- Domain credentials
- NTDS.dit database
- Kerberos secrets
- Trust relationships
- Administrative privileges
- Domain-wide policies

A compromised Domain Controller often means complete domain compromise.

---

# Common Attack Objectives

Attackers attempt to:

- Dump the NTDS.dit database
- Extract password hashes
- Abuse Kerberos
- Steal service tickets
- Modify Group Policy
- Create privileged accounts
- Perform DCSync attacks
- Achieve persistence

---

# Common Attack Surfaces

| Target | Goal |
|--------|------|
| NTDS.dit | Credential extraction |
| LSASS | Credential theft |
| LDAP | Domain enumeration |
| Kerberos | Ticket attacks |
| SYSVOL | GPO abuse |
| DNS | Service discovery |
| Replication | DCSync attacks |

---

# Defensive Perspective

Administrators should:

- Restrict Domain Controller access
- Enable Credential Guard where applicable
- Audit privileged logons
- Monitor replication activity
- Secure LDAP communications
- Apply least privilege
- Separate administrative accounts
- Monitor changes to Group Policy

Domain Controllers should receive the highest level of protection within the enterprise.

---

# Domain Controller vs Member Server

| Domain Controller | Member Server |
|-------------------|---------------|
| Hosts Active Directory | Does not host AD |
| Authenticates users | Uses the DC for authentication |
| Stores NTDS.dit | No AD database |
| Replicates directory data | No replication |
| Enforces domain policies | Receives policies |
| High-value target | Lower-value target |

---

# Why Domain Controllers Matter

Understanding Domain Controllers helps security professionals:

- Analyze authentication flows
- Investigate credential attacks
- Understand Active Directory architecture
- Secure enterprise environments
- Detect lateral movement
- Respond to domain compromises

---

# Key Takeaways

- A Domain Controller is the central authentication and authorization server in Active Directory.
- It stores the Active Directory database (**NTDS.dit**) and provides Kerberos, LDAP, DNS, and replication services.
- Multiple Domain Controllers replicate directory information for redundancy and availability.
- Specialized FSMO roles manage critical domain operations.
- Because it controls identities and trust, the Domain Controller is the highest-value target in most Windows enterprise environments.

---

# Key Insight

> In Active Directory, every trust relationship ultimately depends on the Domain Controller. It is the source of identity, authentication, authorization, and policy enforcement. Protecting the Domain Controller means protecting the security foundation of the entire enterprise.
