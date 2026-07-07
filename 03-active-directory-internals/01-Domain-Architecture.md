# Domain Architecture (Security Perspective)

## Overview

A **Domain** is the fundamental administrative and security boundary in Active Directory. It provides centralized identity management, authentication, authorization, and policy enforcement for all objects within its scope.

From a security perspective:

> A domain is more than a collection of computers it is a trust boundary where identities, permissions, and security policies are centrally managed.

Understanding domain architecture is essential for Active Directory security, privilege escalation, lateral movement, and enterprise defense.

---

# What is a Domain?

A domain is a logical grouping of:

- Users
- Computers
- Groups
- Servers
- Organizational Units (OUs)
- Policies

All objects inside a domain share:

- A common Active Directory database
- Authentication services
- Security policies
- Administrative management

---

# High-Level Architecture

```
                    Domain

    ┌───────────────────────────────────┐
    │                                   │
    │      Domain Controller(s)         │
    │              │                    │
    │      Active Directory             │
    │              │                    │
    │  Users   Computers   Groups       │
    │      │         │         │         │
    │       Organizational Units        │
    │              │                    │
    │        Group Policies             │
    │                                   │
    └───────────────────────────────────┘
```

The Domain Controller acts as the central authority for the domain.

---

# Components of a Domain

A typical domain contains:

| Component | Purpose |
|----------|---------|
| Domain Controllers | Authentication & directory services |
| Users | Identity objects |
| Computers | Joined workstations & servers |
| Groups | Permission management |
| Organizational Units | Administrative organization |
| Group Policy | Security configuration |
| DNS | Name resolution |

---

# Domain Naming

Domains use DNS naming conventions.

Examples:

```
corp.local

company.com

lab.internal
```

The DNS namespace identifies the domain and allows clients to locate Domain Controllers.

---

# Domain Controllers

Every domain contains one or more **Domain Controllers (DCs)**.

Responsibilities include:

- User authentication
- Authorization
- LDAP directory services
- Kerberos services
- Replication
- Policy distribution

If a Domain Controller is compromised, the entire domain is at risk.

---

# Domain Database

Each Domain Controller stores a copy of:

```
NTDS.dit
```

The database contains:

- Users
- Groups
- Password hashes
- Computers
- Policies
- Trust information
- Security descriptors

Changes are replicated between Domain Controllers.

---

# Domain Objects

Everything stored inside Active Directory is an object.

Examples:

- User accounts
- Computer accounts
- Security groups
- Service accounts
- Organizational Units
- Printers
- Shared folders

Each object has:

- Attributes
- Permissions
- Security Identifier (SID)

---

# Authentication Flow

```
User

↓

Domain Controller

↓

Kerberos / NTLM

↓

Identity Verified

↓

Access Token Created

↓

Resource Access
```

The Domain Controller authenticates the identity and issues the information required for authorization.

---

# Authorization

After authentication, Windows determines:

```
What resources may this identity access?
```

Authorization depends on:

- Group membership
- Access Control Lists (ACLs)
- Security descriptors
- Access Tokens

---

# Organizational Units (OUs)

Domains are divided into Organizational Units.

Example:

```
Domain

├── IT

├── HR

├── Finance

└── Servers
```

OUs simplify:

- Administration
- Delegation
- Group Policy application

---

# Group Policy

Group Policy provides centralized configuration.

Examples:

- Password policy
- Firewall settings
- Software deployment
- Security options
- Login scripts

Policies are automatically applied to domain members.

---

# DNS Integration

Active Directory relies heavily on DNS.

DNS allows clients to locate:

- Domain Controllers
- Kerberos services
- LDAP services
- Global Catalog servers

Without DNS, domain authentication cannot function correctly.

---

# Trust Boundary

The domain represents an administrative security boundary.

Within a domain:

- Users trust Domain Controllers
- Computers trust the domain
- Policies are centrally enforced

Cross-domain communication requires explicit trust relationships.

---

# Security Importance

The domain controls:

- User identities
- Computer identities
- Authentication
- Authorization
- Administrative privileges
- Security policies

Compromising a domain often results in enterprise-wide compromise.

---

# Offensive Perspective

Attackers enumerate domains to identify:

- Domain Controllers
- Privileged users
- Administrative groups
- Trust relationships
- Service accounts
- Group Policies
- Misconfigured permissions

The domain serves as the roadmap for privilege escalation and lateral movement.

---

# Common Attack Targets

| Target | Objective |
|---------|-----------|
| Domain Controller | Full domain compromise |
| Domain Admin accounts | Administrative control |
| Service accounts | Credential theft |
| Group Policy | Persistence or code execution |
| DNS | Traffic manipulation |
| ACLs | Privilege escalation |

---

# Defensive Perspective

Administrators should:

- Protect Domain Controllers
- Apply least privilege
- Monitor privileged groups
- Audit ACLs
- Secure DNS infrastructure
- Restrict administrative access
- Monitor authentication events

The domain should be treated as a high-value security asset.

---

# Domain vs Workgroup

| Domain | Workgroup |
|---------|-----------|
| Centralized management | Local management |
| Central authentication | Local accounts |
| Group Policy support | No centralized policies |
| Domain Controllers | None |
| Enterprise scale | Small networks |
| Central trust model | Independent systems |

---

# Why Domain Architecture Matters

Understanding domain architecture enables security professionals to:

- Analyze authentication paths
- Understand privilege boundaries
- Identify attack paths
- Secure enterprise identities
- Perform Active Directory assessments
- Investigate domain-wide incidents

---

# Key Takeaways

- A domain is the core administrative and security boundary in Active Directory.
- Domain Controllers authenticate users, authorize access, and replicate directory data.
- Active Directory stores domain information inside the **NTDS.dit** database.
- Domains integrate tightly with DNS, Kerberos, LDAP, and Group Policy.
- All users, computers, and resources rely on the domain's trust model.
- Compromising the domain often leads to complete control over enterprise resources.

---

# Key Insight

> A Windows domain is fundamentally a **trust architecture**. Every authentication request, access decision, and administrative action flows through this centralized trust model. For attackers, the domain is the primary target; for defenders, it is the most critical asset to protect.
