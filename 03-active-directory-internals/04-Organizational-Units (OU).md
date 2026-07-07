# Organizational Units (OU) (Security Perspective)

## Overview

An **Organizational Unit (OU)** is a logical container within an Active Directory domain used to organize objects, delegate administrative control, and apply Group Policy Objects (GPOs).

From a security perspective:

> Organizational Units are not security boundaries they are administrative boundaries. Misunderstanding this distinction can lead to privilege escalation and improper access control.

Understanding OUs is essential for:

- Active Directory Administration
- Group Policy Management
- Delegation of Control
- Enterprise Security
- Active Directory Assessments

---

# What is an Organizational Unit?

An Organizational Unit is a container that can hold:

- Users
- Computers
- Groups
- Printers
- Shared folders
- Other Organizational Units

Unlike domains, OUs exist only inside a single domain.

---

# High-Level Structure

```
Domain

│

├── Users

├── Workstations

├── Servers

├── IT

│     ├── Administrators
│     ├── Helpdesk
│     └── Service Accounts

├── HR

└── Finance
```

OUs create a logical administrative hierarchy.

---

# Purpose of Organizational Units

OUs are designed to:

- Organize directory objects
- Delegate administration
- Apply Group Policies
- Simplify management

They do **not** create authentication or trust boundaries.

---

# Objects Inside an OU

Typical objects include:

| Object | Example |
|---------|----------|
| User | Alice |
| Computer | HR-PC01 |
| Group | Finance Users |
| Service Account | SQLService |
| Nested OU | IT\Admins |

---

# Nested Organizational Units

OUs can contain other OUs.

Example:

```
IT

├── Administrators

├── Servers

├── Workstations

└── Service Accounts
```

This hierarchy allows administrators to organize environments efficiently.

---

# Delegation of Administration

One of the primary uses of OUs is administrative delegation.

Example:

```
Domain Admin

↓

IT OU Administrator

↓

Helpdesk OU Administrator
```

Delegation allows administrators to manage specific OUs without granting full Domain Admin privileges.

---

# Group Policy Application

Group Policies are commonly linked to OUs.

Example:

```
Finance OU

↓

Finance Security Policy

↓

Finance Computers
```

Policies applied at the OU level affect all objects inside that OU unless inheritance is modified.

---

# Policy Processing Order

Group Policy follows the **LSDOU** processing order:

```
Local

↓

Site

↓

Domain

↓

Organizational Unit
```

When multiple GPOs apply, policies processed later may override earlier settings.

---

# Inheritance

By default:

- Parent OU policies apply to child OUs.
- Child OUs inherit policies from higher levels.

Example:

```
Domain

↓

IT OU

↓

Administrators OU
```

Policies linked to the domain automatically apply to the child OUs unless inheritance is blocked.

---

# Block Inheritance

Administrators may configure an OU to block inherited Group Policies.

Purpose:

- Prevent unwanted policies
- Apply specialized configurations

Improper use may result in inconsistent security settings.

---

# Enforced Policies

Some Group Policies can be marked as **Enforced**.

Characteristics:

- Cannot be overridden by child OUs
- Continue to apply even if inheritance is blocked

This mechanism is commonly used for enterprise-wide security policies.

---

# Security Boundaries

It is important to understand that:

**An Organizational Unit is NOT a security boundary.**

It provides:

- Administrative organization
- Delegation
- Group Policy targeting

It does **not** isolate:

- Authentication
- Authorization
- Trust relationships

The **domain** remains the true security boundary.

---

# Security Importance

OUs influence security because they determine:

- Administrative scope
- Policy application
- User configuration
- Computer configuration

Poor OU design often results in:

- Excessive administrative privileges
- Misconfigured policies
- Weak operational security

---

# Offensive Perspective

Attackers analyze OUs to identify:

- Administrative delegation
- High-value systems
- Privileged users
- Group Policy targets
- Organizational structure

The OU hierarchy often reveals how the enterprise is managed.

---

# Common Attack Opportunities

Attackers look for:

- Weak delegation permissions
- Misconfigured GPO links
- Writable Organizational Units
- Unauthorized object creation
- Over-privileged delegated administrators

Although OUs are not security boundaries, misconfigurations can enable privilege escalation.

---

# Common Enumeration Targets

| Target | Purpose |
|---------|---------|
| OU hierarchy | Understand enterprise structure |
| Delegated permissions | Find escalation paths |
| Linked GPOs | Identify applied policies |
| Administrative OUs | Locate privileged accounts |
| Server OUs | Identify critical infrastructure |

---

# Defensive Perspective

Administrators should:

- Design a clear OU hierarchy
- Delegate only required permissions
- Apply least privilege
- Audit delegated rights
- Review GPO inheritance
- Avoid unnecessary nesting
- Separate privileged accounts from standard users

A well-designed OU structure simplifies administration and reduces security risk.

---

# Organizational Unit vs Domain

| Organizational Unit | Domain |
|---------------------|--------|
| Administrative container | Security boundary |
| Holds directory objects | Holds OUs and objects |
| Supports delegation | Provides authentication |
| Receives Group Policies | Contains Domain Controllers |
| No trust boundary | Authentication boundary |

---

# Example Enterprise

```
company.local

│

├── Users

├── Workstations

├── Servers

├── IT

│     ├── Admins

│     ├── Helpdesk

│     └── Service Accounts

├── HR

└── Finance
```

Each department receives:

- Separate administration
- Dedicated Group Policies
- Independent object organization

---

# Why Organizational Units Matter

Understanding OUs enables security professionals to:

- Analyze administrative delegation
- Understand Group Policy targeting
- Identify privilege escalation opportunities
- Assess Active Directory design
- Review enterprise administration

---

# Key Takeaways

- Organizational Units are logical containers within a domain.
- They organize Active Directory objects and simplify administration.
- OUs enable delegated administration and Group Policy application.
- OUs are **not** security boundaries.
- Poor delegation or GPO configuration can create privilege escalation paths.
- A well-designed OU hierarchy improves both security and manageability.

---

# Key Insight

> Organizational Units exist to organize administration not to enforce security. While they play a critical role in delegation and Group Policy management, the true security boundary remains the domain. Understanding this distinction is essential when assessing Active Directory environments from both offensive and defensive perspectives.
