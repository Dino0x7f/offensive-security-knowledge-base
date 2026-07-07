# Group Policy (GPO) (Security Perspective)

## Overview

**Group Policy (GPO)** is Microsoft's centralized configuration management system used to control security settings, operating system behavior, user environments, and software deployment across an Active Directory domain.

From a security perspective:

> Group Policy is one of the most powerful administrative mechanisms in Active Directory. A single malicious or misconfigured GPO can impact thousands of systems simultaneously.

Understanding Group Policy is essential for:

- Active Directory Security
- Enterprise Administration
- Privilege Escalation
- Lateral Movement
- Persistence
- Incident Response

---

# What is a Group Policy?

A Group Policy is a collection of configuration settings applied to:

- Users
- Computers

These settings are managed centrally through Active Directory and automatically applied by domain-joined systems.

---

# High-Level Architecture

```
Administrator

↓

Group Policy Management

↓

Domain Controller

↓

SYSVOL

↓

Client Computer

↓

Policy Applied
```

The Domain Controller stores and distributes Group Policies to clients.

---

# Components of a GPO

Every Group Policy consists of two components:

| Component | Purpose |
|-----------|---------|
| Group Policy Container (GPC) | Stores policy metadata inside Active Directory |
| Group Policy Template (GPT) | Stores policy files inside SYSVOL |

Together they define and deliver policy settings.

---

# GPO Storage

Policies are stored in two locations.

### Active Directory

Stores:

- GUID
- Version
- Links
- Metadata

Database:

```
NTDS.dit
```

---

### SYSVOL

Stores:

- Scripts
- Security templates
- Registry policies
- Administrative Templates

Default path:

```
C:\Windows\SYSVOL\
```

---

# GPO Processing

Policies are applied in the following order:

```
Local

↓

Site

↓

Domain

↓

Organizational Unit
```

This order is commonly known as:

```
LSDOU
```

Policies processed later may override previous settings.

---

# Types of Policies

Group Policy can configure:

## Computer Configuration

Applies before user logon.

Examples:

- Firewall
- Services
- Registry
- Security Settings
- Startup Scripts

---

## User Configuration

Applies after user authentication.

Examples:

- Desktop Settings
- Login Scripts
- Drive Mapping
- Folder Redirection
- Control Panel Restrictions

---

# Group Policy Objects (GPO)

A GPO is identified by a unique GUID.

Example:

```
{31B2F340-016D-11D2-945F-00C04FB984F9}
```

Multiple Organizational Units may link to the same GPO.

---

# GPO Linking

Policies can be linked to:

- Sites
- Domains
- Organizational Units

Example:

```
Domain

↓

IT OU

↓

IT Workstations

↓

Security Policy
```

Objects inside the OU receive the policy.

---

# Inheritance

Policies are inherited automatically.

Example:

```
Domain

↓

Servers OU

↓

Database Servers OU
```

Policies applied to the Domain also affect child OUs unless inheritance is blocked.

---

# Block Inheritance

Administrators may prevent inherited policies from applying.

Purpose:

- Specialized configurations
- Department-specific policies

Misuse can weaken enterprise security.

---

# Enforced Policies

Some GPOs are marked as:

```
Enforced
```

Characteristics:

- Cannot be overridden
- Continue to apply even if inheritance is blocked

Commonly used for critical enterprise security policies.

---

# Security Settings Managed by GPO

Examples include:

- Password Policy
- Account Lockout Policy
- Windows Firewall
- Defender Settings
- BitLocker
- Audit Policy
- User Rights Assignment
- Restricted Groups

These settings directly affect enterprise security.

---

# Scripts

Group Policy supports:

- Startup Scripts
- Shutdown Scripts
- Logon Scripts
- Logoff Scripts

Scripts execute automatically according to policy.

---

# Software Deployment

Administrators can deploy applications through GPO.

Example:

```
Domain

↓

Software Policy

↓

Microsoft Office

↓

Installed Automatically
```

This simplifies enterprise software management.

---

# Security Importance

Group Policy controls:

- Authentication behavior
- Password requirements
- Administrative privileges
- Windows Defender
- Firewall rules
- Logging
- User restrictions

Because of its broad influence, GPO is a high-value administrative mechanism.

---

# SYSVOL

SYSVOL is replicated across Domain Controllers.

Contains:

- Group Policy Templates
- Login Scripts
- Administrative Templates

Clients retrieve policies from SYSVOL during policy refresh.

---

# Policy Refresh

Group Policy is updated:

- During system startup
- During user logon
- At regular background intervals
- Manually using:

```
gpupdate
```

Clients periodically check for policy changes.

---

# Offensive Perspective

Attackers target Group Policy because it enables centralized execution.

Objectives include:

- Execute malicious scripts
- Deploy malware
- Add administrators
- Modify security settings
- Disable defenses
- Establish persistence

A compromised GPO can affect every linked system.

---

# Common Attack Targets

| Target | Objective |
|---------|-----------|
| SYSVOL | Modify policy files |
| GPO ACLs | Gain modification rights |
| Startup Scripts | Code execution |
| Logon Scripts | Persistence |
| Restricted Groups | Privilege escalation |
| Scheduled Tasks | Remote execution |

---

# Common Misconfigurations

Examples include:

- Writable GPOs
- Excessive delegation
- Weak SYSVOL permissions
- Unused legacy policies
- Overly broad GPO links

These issues may provide privilege escalation opportunities.

---

# Defensive Perspective

Administrators should:

- Restrict GPO editing permissions
- Audit GPO changes
- Monitor SYSVOL
- Protect privileged GPOs
- Remove obsolete policies
- Review delegated administration
- Enable Advanced Auditing

Changes to Group Policy should be considered high-risk events.

---

# GPO vs OU

| GPO | OU |
|------|----|
| Configuration object | Administrative container |
| Applies policies | Organizes objects |
| Can link to multiple OUs | Holds users and computers |
| Controls system behavior | Controls administration |

---

# Why Group Policy Matters

Understanding Group Policy enables security professionals to:

- Analyze enterprise configuration
- Investigate privilege escalation
- Understand Windows security enforcement
- Detect persistence mechanisms
- Secure Active Directory environments
- Perform GPO security assessments

---

# Key Takeaways

- Group Policy provides centralized management for Windows environments.
- Every GPO consists of a Group Policy Container (GPC) and Group Policy Template (GPT).
- Policies are processed using the LSDOU order.
- GPOs configure security, software, scripts, and operating system behavior.
- SYSVOL stores policy files and is replicated across Domain Controllers.
- Misconfigured or compromised GPOs can affect the entire enterprise.

---

# Key Insight

> Group Policy is one of the most powerful management mechanisms in Active Directory. It enables administrators to enforce security consistently across thousands of systems—but the same capability makes it a prime target for attackers seeking privilege escalation, persistence, or large-scale compromise. Protecting GPO integrity is essential to protecting the enterprise.
