# Active Directory Overview (Security Perspective)

## Overview

Active Directory (AD) is Microsoft's centralized directory service for managing identities, authentication, authorization, and resources within Windows enterprise environments.

From a security perspective:

> Active Directory is not just a directory service it is the trust infrastructure of a Windows enterprise. Compromising Active Directory often means compromising the entire organization.

Understanding Active Directory is fundamental for:

- Enterprise Penetration Testing
- Red Team Operations
- Active Directory Security
- Incident Response
- Malware Analysis
- Privilege Escalation
- Lateral Movement

---

# What is Active Directory?

Active Directory is a directory service that stores information about:

- Users
- Computers
- Groups
- Servers
- Printers
- Policies
- Domains
- Trust relationships

It provides centralized management for authentication and authorization across an enterprise network.

---

# Why Active Directory Exists

Before Active Directory:

- Local accounts
- Independent authentication
- Difficult administration
- No centralized policy management

With Active Directory:

- Centralized authentication
- Centralized authorization
- Unified policy management
- Scalable administration
- Secure resource sharing

---

# High-Level Architecture

```
Enterprise Network

        │

        ▼

Active Directory

│
├── Domains
├── Domain Controllers
├── Users
├── Computers
├── Groups
├── Organizational Units
├── Policies
└── Services
```

Active Directory becomes the central authority for identity and trust.

---

# Core Responsibilities

Active Directory manages:

- Identity
- Authentication
- Authorization
- Policy enforcement
- Resource management
- Trust relationships

Every Windows domain relies on Active Directory for these functions.

---

# Core Components

The major components include:

| Component | Purpose |
|----------|----------|
| Domain | Administrative boundary |
| Domain Controller (DC) | Hosts Active Directory services |
| Forest | Highest security boundary |
| Tree | Collection of related domains |
| Organizational Unit (OU) | Administrative organization |
| Group Policy (GPO) | Centralized configuration |
| DNS | Name resolution |
| NTDS.dit | Active Directory database |

---

# Identity Model

Every object inside Active Directory has an identity.

Examples include:

- Users
- Computers
- Service Accounts
- Groups

Each object contains:

- Attributes
- Permissions
- Security Identifier (SID)

---

# Authentication

Active Directory authenticates users using protocols such as:

- Kerberos
- NTLM

Authentication answers:

```
Who are you?
```

---

# Authorization

After authentication:

Active Directory determines:

```
What are you allowed to access?
```

Authorization depends on:

- Group membership
- Access Control Lists (ACLs)
- Security descriptors
- Access Tokens

---

# Trust Model

Everything inside Active Directory is built around trust.

Examples:

```
User

↓

Domain

↓

Domain Controller

↓

Resource
```

Trust relationships also exist between:

- Domains
- Forests
- External organizations

---

# Domain Controllers

A Domain Controller is responsible for:

- Authentication
- Authorization
- Directory replication
- Policy distribution
- Kerberos services
- LDAP services

It is the most critical server in an Active Directory environment.

---

# Active Directory Database

The directory information is stored inside:

```
NTDS.dit
```

The database contains:

- Users
- Groups
- Password hashes
- Policies
- Security information
- Object relationships

---

# Active Directory Services

Important services include:

| Service | Purpose |
|---------|----------|
| LDAP | Directory queries |
| Kerberos | Authentication |
| DNS | Name resolution |
| SMB | File sharing |
| RPC | Remote communication |
| Group Policy | Configuration management |

---

# Security Importance

Active Directory controls:

- User identities
- Administrative privileges
- Enterprise authentication
- Access to servers
- Access to workstations
- Resource permissions

Compromising Active Directory usually leads to complete enterprise compromise.

---

# Attacker Perspective

Attackers view Active Directory as:

- An identity database
- A privilege graph
- A trust network
- An authentication system
- A lateral movement platform

Their objective is rarely to compromise a single machine—it is to compromise the domain.

---

# Common Attack Objectives

Attackers attempt to:

- Enumerate the domain
- Discover privileged users
- Steal credentials
- Abuse Kerberos
- Exploit misconfigured ACLs
- Escalate privileges
- Move laterally
- Gain Domain Admin privileges

---

# Common Attack Surfaces

Examples include:

- Weak passwords
- NTLM
- Kerberos delegation
- Group Policy
- Service accounts
- Misconfigured permissions
- Active Directory Certificate Services (AD CS)
- Trust relationships

---

# Defensive Perspective

Administrators use Active Directory to:

- Centralize identity management
- Enforce least privilege
- Apply security policies
- Monitor authentication
- Detect suspicious activity
- Secure enterprise resources

---

# Learning Path

This section will cover:

1. Domain Architecture
2. Domain Controllers
3. Forests, Trees, and Domains
4. Organizational Units
5. NTDS Database
6. Active Directory Objects
7. Security Identifiers (SID)
8. Groups and Permissions
9. Group Policy
10. DNS Integration
11. Kerberos Internals
12. NTLM Internals
13. Authentication Flow
14. Access Tokens
15. Trust Relationships
16. Replication
17. Active Directory Certificate Services (AD CS)
18. Common Attack Paths
19. Active Directory Cheatsheet

---

# Prerequisites

Before studying this section, you should understand:

- Windows Fundamentals
- Windows Internals
- Networking Basics
- Authentication Concepts
- Operating System Security

---

# Next Step

After completing **Active Directory Internals**, continue with:

- Active Directory Attacks
- Windows Privilege Escalation
- Credential Attacks
- Lateral Movement
- Persistence
- Defense & Detection

---

# Key Insight

> Active Directory is the security backbone of most Windows enterprise environments. It is not simply a database of users—it is a distributed trust system that controls identity, authentication, authorization, and access across the organization. Understanding its internal architecture is essential for both securing enterprise networks and understanding how sophisticated attackers achieve domain-wide compromise.
