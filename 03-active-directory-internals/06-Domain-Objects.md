# Domain Objects (Security Perspective)

## Overview

Everything stored in Active Directory is represented as an **object**. Users, computers, groups, printers, service accounts, and even Organizational Units (OUs) are all objects with attributes and security information.

From a security perspective:

> Objects are the building blocks of Active Directory. Every authentication decision, permission assignment, and trust relationship revolves around directory objects.

Understanding domain objects is essential for:

- Active Directory Security
- Identity Management
- Privilege Escalation
- Access Control
- Active Directory Enumeration
- Incident Response

---

# What is a Domain Object?

A domain object is an entity stored inside the Active Directory database.

Every object has:

- A unique identity
- A set of attributes
- A Security Identifier (SID)
- Permissions
- An Object Class

Examples include:

- User
- Computer
- Group
- Organizational Unit
- Printer
- Shared Folder
- Service Account

---

# High-Level Architecture

```
Active Directory

│

├── Users

├── Computers

├── Groups

├── Organizational Units

├── Printers

├── Service Accounts

└── Shared Resources
```

Every object is stored inside **NTDS.dit**.

---

# Object Structure

Each object contains:

```
Object

│

├── Name

├── Distinguished Name (DN)

├── SID

├── GUID

├── Attributes

├── ACL

└── Object Class
```

These properties uniquely identify and secure the object.

---

# Object Classes

Every object belongs to a class.

Common classes include:

| Object Class | Purpose |
|--------------|---------|
| User | User account |
| Computer | Domain-joined device |
| Group | Permission management |
| Organizational Unit | Administrative container |
| Contact | Directory information |
| Printer | Shared printer |
| Container | Logical grouping |

The object class determines which attributes the object can have.

---

# User Objects

A user object represents a domain identity.

Common attributes include:

- Username
- Display Name
- SID
- Group Membership
- Password Metadata
- Account Status
- Logon Information

Users authenticate to the domain using these objects.

---

# Computer Objects

Every domain-joined computer has its own object.

Contains information such as:

- Computer name
- Operating System
- SID
- Service Principal Names (SPNs)
- Machine account password

Computers authenticate just like users.

---

# Group Objects

Groups simplify permission management.

Examples:

- Domain Admins
- Enterprise Admins
- Helpdesk
- HR Users

Groups are assigned permissions instead of individual users.

---

# Organizational Unit Objects

OUs organize directory objects.

Used for:

- Delegation
- Group Policy
- Administrative organization

They are containers—not security boundaries.

---

# Service Accounts

Some services require domain identities.

Examples:

- SQL Server
- IIS
- Exchange
- Scheduled Tasks

These accounts often possess elevated privileges and are attractive targets for attackers.

---

# Security Identifier (SID)

Every security object receives a unique SID.

Example:

```
S-1-5-21-...
```

Windows uses the SID—not the username—to make authorization decisions.

---

# Globally Unique Identifier (GUID)

Every object also has a GUID.

Characteristics:

- Globally unique
- Never changes
- Used internally by Active Directory

Unlike names, GUIDs remain constant even if an object is renamed.

---

# Distinguished Name (DN)

Each object has a Distinguished Name that identifies its location within the directory.

Example:

```
CN=Alice,CN=Users,DC=corp,DC=local
```

The DN describes the object's path in the directory hierarchy.

---

# Object Attributes

Every object stores attributes.

Examples for a user:

| Attribute | Description |
|------------|-------------|
| sAMAccountName | Logon name |
| displayName | Full name |
| mail | Email address |
| memberOf | Group memberships |
| objectSID | Security Identifier |
| objectGUID | Unique identifier |

Attributes define the object's properties.

---

# Object Security

Objects are protected by:

- Access Control Lists (ACLs)
- Security Descriptors
- Ownership
- Inheritance

Permissions determine who can:

- Read
- Modify
- Delete
- Delegate
- Change permissions

---

# Object Relationships

Objects reference one another.

Example:

```
User

↓

Member Of

↓

Group

↓

Permissions

↓

Resource
```

These relationships define authorization within the domain.

---

# Object Lifecycle

Typical lifecycle:

```
Creation

↓

Modification

↓

Authentication

↓

Authorization

↓

Deletion
```

Throughout its lifecycle, an object retains its GUID and security information.

---

# Security Importance

Objects define:

- Identity
- Privileges
- Permissions
- Authentication
- Group membership
- Administrative delegation

Every access decision in Active Directory ultimately involves one or more objects.

---

# Offensive Perspective

Attackers enumerate objects to discover:

- Privileged users
- Administrative groups
- Service accounts
- Domain Controllers
- Computers
- Delegation settings
- Misconfigured permissions

Object relationships reveal potential privilege escalation paths.

---

# High-Value Objects

| Object | Reason |
|----------|--------|
| Domain Admin | Full domain control |
| Enterprise Admin | Forest-wide control |
| Domain Controller | Authentication authority |
| Service Account | Credential theft target |
| Group Policy Object | Persistence opportunities |
| Computer Account | Lateral movement |

---

# Common Enumeration Targets

Attackers commonly identify:

- Users
- Computers
- Groups
- OUs
- Service Accounts
- SPNs
- ACLs
- Administrative memberships

This information is used to build attack paths through the domain.

---

# Defensive Perspective

Administrators should:

- Apply least privilege
- Audit object permissions
- Monitor object creation
- Protect privileged accounts
- Secure service accounts
- Review delegated permissions
- Remove unused objects

Proper object management significantly reduces attack surface.

---

# Why Domain Objects Matter

Understanding domain objects enables security professionals to:

- Analyze authentication
- Understand authorization
- Investigate privilege escalation
- Review Active Directory permissions
- Assess enterprise security posture
- Perform directory-based threat hunting

---

# Key Takeaways

- Everything in Active Directory is represented as an object.
- Objects contain attributes, identifiers, and security information.
- Users, computers, groups, and OUs are all domain objects.
- Every security object has a SID and a GUID.
- Object permissions are enforced through ACLs and security descriptors.
- Object relationships form the foundation of Active Directory's trust and authorization model.

---

# Key Insight

> Active Directory is fundamentally an object-based directory. Authentication identifies objects, authorization evaluates their relationships, and security is enforced through the permissions attached to them. Understanding domain objects means understanding how trust is represented throughout the entire enterprise.
