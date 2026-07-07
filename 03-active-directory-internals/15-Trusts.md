# Trusts (Security Perspective)

## Overview

A **Trust** is a relationship between two security boundaries that allows authentication requests to cross domain or forest boundaries.

In Active Directory, trusts enable users from one domain to access resources located in another domain without requiring separate accounts.

From a security perspective:

> Trusts extend authentication beyond a single domain. While they improve interoperability, they also expand the attack surface. Many enterprise compromises spread by abusing trust relationships rather than exploiting vulnerabilities.

Understanding trusts is essential for:

- Active Directory Security
- Forest Architecture
- Cross-Domain Authentication
- Privilege Escalation
- Lateral Movement
- Red Team Operations
- Incident Response

---

# What is a Trust?

A trust is a secure authentication relationship between two security boundaries.

It allows:

- User authentication
- Resource access
- Cross-domain authorization

without duplicating user accounts.

---

# High-Level Architecture

```
Domain A

↓

Trust Relationship

↓

Domain B

↓

Resource Access
```

The trusting domain accepts authentication from the trusted domain.

---

# Why Trusts Exist

Without trusts:

```
User

↓

Domain A

✗

Cannot Access

↓

Domain B
```

With a trust:

```
User

↓

Domain A

↓

Trust

↓

Domain B

↓

Access Resource
```

---

# Authentication Across a Trust

```
User

↓

Domain A

↓

Domain Controller

↓

Trust Validation

↓

Domain B

↓

Access Granted
```

Authentication remains centralized while authorization occurs in the resource domain.

---

# Trust Direction

Trusts can be:

- One-Way
- Two-Way

---

## One-Way Trust

```
Domain A

────────▶

Domain B
```

Meaning:

- Domain B trusts Domain A.
- Users from Domain A may access Domain B resources (if authorized).
- The reverse is not true.

---

## Two-Way Trust

```
Domain A

◀────────▶

Domain B
```

Both domains trust one another.

Authentication works in both directions.

---

# Trust Types

Active Directory supports multiple trust types.

| Trust Type | Purpose |
|------------|----------|
| Parent-Child | Between parent and child domains |
| Tree-Root | Between domain trees |
| External | Between separate domains |
| Forest | Between separate forests |
| Shortcut | Optimize authentication |
| Realm | Active Directory ↔ Kerberos realm |

---

# Parent-Child Trust

Automatically created.

Example:

```
corp.local

↓

sales.corp.local
```

Characteristics:

- Automatic
- Two-way
- Transitive

---

# Tree-Root Trust

Created when multiple trees exist in the same forest.

Example:

```
corp.local

⇄

company.net
```

Both belong to the same forest.

---

# Forest Trust

Created between separate forests.

Example:

```
Forest A

⇄

Forest B
```

Allows authentication across forests.

Usually:

- Two-way
- Transitive

---

# External Trust

Used between domains in different forests.

Characteristics:

- Manual
- Non-transitive
- Limited scope

---

# Shortcut Trust

Shortcut trusts reduce authentication latency.

Instead of:

```
Domain A

↓

Domain B

↓

Domain C

↓

Domain D
```

Authentication becomes:

```
Domain A

↓

Shortcut

↓

Domain D
```

---

# Realm Trust

Allows interoperability with:

- MIT Kerberos
- UNIX Kerberos Realms

Used in heterogeneous environments.

---

# Transitive Trust

A transitive trust extends automatically.

Example:

```
Domain A

⇄

Domain B

⇄

Domain C
```

Result:

```
Domain A

Automatically Trusts

Domain C
```

---

# Non-Transitive Trust

Only the participating domains trust one another.

```
Domain A

⇄

Domain B

✗

Domain C
```

No trust extension occurs.

---

# Trust Authentication

Authentication across trusts typically uses:

```
Kerberos

↓

Referral Tickets

↓

Target Domain
```

Kerberos referral tickets allow authentication to continue across trusted domains.

---

# Trust Boundaries

Although trusts allow authentication:

Each domain remains its own:

- Administrative boundary
- Security boundary

Trusts do **not** automatically grant administrative privileges.

---

# Security Importance

Trusts enable:

- Cross-domain authentication
- Enterprise collaboration
- Forest-wide access
- Resource sharing

They also expand the scope of potential compromise.

---

# Offensive Perspective

Attackers enumerate trusts to identify:

- Additional domains
- Additional forests
- Administrative relationships
- Privileged users
- Cross-domain attack paths

Trusts often reveal opportunities for lateral movement.

---

# Common Attack Targets

| Target | Objective |
|----------|-----------|
| Forest Trust | Forest compromise |
| External Trust | Cross-domain access |
| Domain Trust | Lateral movement |
| Trust Keys | Authentication abuse |
| Kerberos Referrals | Ticket abuse |

---

# Trust Enumeration

Attackers commonly identify:

- Trust direction
- Trust type
- Trust attributes
- Trusted forests
- Trusted domains
- Transitivity
- Authentication scope

This information is used to map enterprise relationships.

---

# Common Misconfigurations

Examples include:

- Excessive trust permissions
- Legacy external trusts
- Weak selective authentication
- Unnecessary forest trusts
- Poor administrative separation

Misconfigured trusts increase attack surface.

---

# Defensive Perspective

Administrators should:

- Create trusts only when necessary
- Review trust relationships regularly
- Remove obsolete trusts
- Enable selective authentication when appropriate
- Monitor trust modifications
- Audit cross-domain administrative access
- Separate privileged administration between domains

Trusts should be treated as high-value security relationships.

---

# Trust vs Forest

| Trust | Forest |
|--------|---------|
| Authentication relationship | Collection of domains |
| Connects security boundaries | Defines enterprise boundary |
| Optional | Core AD structure |
| Enables resource sharing | Provides centralized administration |

---

# Why Trusts Matter

Understanding trusts enables security professionals to:

- Analyze enterprise authentication
- Understand cross-domain access
- Detect lateral movement
- Assess Active Directory architecture
- Investigate privilege escalation
- Secure multi-domain environments

---

# Key Takeaways

- Trusts allow authentication across domains and forests.
- Trusts may be one-way or two-way.
- Trusts may be transitive or non-transitive.
- Active Directory supports Parent-Child, Tree-Root, Forest, External, Shortcut, and Realm trusts.
- Kerberos referral tickets are commonly used during cross-domain authentication.
- Trusts improve collaboration but also expand the attack surface.

---

# Key Insight

> Trusts are authentication bridges between security boundaries. They allow identities to move across domains and forests while preserving centralized authentication. For defenders, they enable enterprise interoperability; for attackers, they often represent the pathways used to expand an initial compromise into a domain-wide or forest-wide breach.
