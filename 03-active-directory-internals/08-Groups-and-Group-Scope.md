# Groups and Group Scope (Security Perspective)

## Overview

Groups are one of the most important components in Active Directory. Rather than assigning permissions directly to individual users, permissions are assigned to groups, and users become members of those groups.

From a security perspective:

> Groups define authorization. While authentication identifies who you are, group membership determines what you are allowed to access.

Understanding groups is essential for:

- Active Directory Security
- Authorization
- Privilege Management
- Least Privilege
- Privilege Escalation
- Incident Response

---

# What is a Group?

A group is an Active Directory object that contains one or more members.

Members can include:

- Users
- Computers
- Service Accounts
- Other Groups

Groups simplify permission management by allowing administrators to assign permissions once instead of individually.

---

# High-Level Architecture

```
User

↓

Member Of

↓

Security Group

↓

Access Control List (ACL)

↓

Resource Access
```

Windows evaluates group membership during authorization.

---

# Why Groups Exist

Without groups:

```
Permission

↓

Every User Individually
```

With groups:

```
Users

↓

Group

↓

Permissions

↓

Resource
```

Groups make permission management scalable and consistent.

---

# Types of Groups

Active Directory defines two categories of groups.

## Security Groups

Used for:

- Authorization
- Access Control
- ACLs
- Privilege Assignment

Examples:

- Domain Admins
- Backup Operators
- HR Users

---

## Distribution Groups

Used only for:

- Email distribution
- Microsoft Exchange

They are **not** used during authorization.

---

# Group Scope

Every security group has a scope.

There are three primary scopes:

| Scope | Purpose |
|--------|----------|
| Global | Organize users |
| Domain Local | Assign permissions |
| Universal | Multi-domain environments |

---

# Global Groups

Global Groups contain objects from the same domain.

Example:

```
corp.local

↓

IT Users

↓

Alice

Bob

Charlie
```

Characteristics:

- Members from same domain
- Can be granted permissions anywhere
- Commonly represent departments or teams

---

# Domain Local Groups

Domain Local Groups are primarily used for assigning permissions to resources inside their own domain.

Example:

```
Finance Share

↓

Finance Access Group

↓

Global Groups

↓

Users
```

Characteristics:

- Resource-focused
- Can contain members from trusted domains
- Permissions remain local to the domain

---

# Universal Groups

Universal Groups operate across the entire forest.

Characteristics:

- Members from multiple domains
- Permissions across multiple domains
- Stored in the Global Catalog

Common in large enterprise environments.

---

# Group Nesting

Groups can contain other groups.

Example:

```
User

↓

Global Group

↓

Universal Group

↓

Domain Local Group

↓

Resource
```

This simplifies enterprise administration.

---

# AGDLP Model

Microsoft recommends:

```
Accounts

↓

Global Groups

↓

Domain Local Groups

↓

Permissions
```

Known as:

```
AGDLP
```

Meaning:

- A = Accounts
- G = Global Groups
- DL = Domain Local Groups
- P = Permissions

---

# AGUDLP Model

In multi-domain forests:

```
Accounts

↓

Global Groups

↓

Universal Groups

↓

Domain Local Groups

↓

Permissions
```

This model improves scalability across domains.

---

# Built-in Security Groups

Active Directory includes predefined groups.

| Group | Purpose |
|--------|----------|
| Domain Admins | Domain administration |
| Enterprise Admins | Forest administration |
| Schema Admins | Schema modification |
| Account Operators | User management |
| Backup Operators | Backup privileges |
| Server Operators | Server administration |
| Print Operators | Printer management |

These groups possess powerful privileges.

---

# Privileged Groups

High-value groups include:

```
Enterprise Admins

↓

Domain Admins

↓

Administrators

↓

Schema Admins
```

Membership should be tightly controlled.

---

# Authorization Process

After authentication:

```
User

↓

Access Token

↓

User SID

+

Group SIDs

↓

ACL Evaluation

↓

Access Decision
```

Windows evaluates all group memberships before granting access.

---

# Group Membership

Users may belong to multiple groups.

Example:

```
Alice

↓

HR Users

↓

VPN Users

↓

Remote Desktop Users

↓

Backup Operators
```

Effective permissions are the combination of all group memberships.

---

# Security Importance

Groups define:

- Administrative privileges
- Resource permissions
- Authentication rights
- Authorization boundaries

Most enterprise authorization decisions rely on groups.

---

# Offensive Perspective

Attackers enumerate groups to identify:

- Domain Admins
- Enterprise Admins
- Backup Operators
- Service Administrators
- Helpdesk Groups
- Delegated Administrators

Group memberships often reveal privilege escalation paths.

---

# Common Attack Targets

| Group | Objective |
|--------|-----------|
| Domain Admins | Domain compromise |
| Enterprise Admins | Forest compromise |
| Backup Operators | Sensitive data access |
| Account Operators | User creation |
| Server Operators | Service control |
| DNS Admins | Infrastructure compromise |

---

# Group Enumeration

Attackers commonly enumerate:

- Group names
- Members
- Nested groups
- Privileged memberships
- Delegated administration
- ACL permissions

This information is used to build attack graphs.

---

# Defensive Perspective

Administrators should:

- Apply least privilege
- Review privileged memberships regularly
- Remove unused groups
- Audit nested groups
- Limit Domain Admin usage
- Monitor changes to privileged groups

Groups should represent roles—not individuals.

---

# Common Misconfigurations

Examples include:

- Excessive Domain Admin members
- Deep group nesting
- Unused privileged groups
- Broad administrative delegation
- Forgotten service accounts in privileged groups

These issues increase attack surface.

---

# Global vs Domain Local vs Universal

| Feature | Global | Domain Local | Universal |
|----------|--------|--------------|------------|
| Members from same domain | Yes | No | No |
| Used for permissions | Indirectly | Yes | Yes |
| Multi-domain support | No | Yes | Yes |
| Stored in Global Catalog | No | No | Yes |

---

# Why Groups Matter

Understanding groups enables security professionals to:

- Analyze authorization
- Identify privilege escalation paths
- Review enterprise permissions
- Investigate administrative access
- Assess Active Directory security
- Implement least privilege

---

# Key Takeaways

- Groups simplify authorization by assigning permissions collectively.
- Security Groups participate in authorization; Distribution Groups do not.
- Active Directory defines three scopes: Global, Domain Local, and Universal.
- Microsoft's recommended models are **AGDLP** and **AGUDLP**.
- Group memberships are included in Access Tokens during logon.
- Privileged groups represent some of the highest-value targets in Active Directory.

---

# Key Insight

> Authentication answers **who you are**, but group membership determines **what you can do**. In Active Directory, groups are the foundation of authorization, making them one of the most critical components for both enterprise administration and security.
