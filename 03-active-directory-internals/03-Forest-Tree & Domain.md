# Forest, Tree & Domain (Security Perspective)

## Overview

Active Directory is organized into a hierarchical structure composed of **Domains**, **Trees**, and **Forests**. These logical structures define administrative boundaries, authentication relationships, and trust between different parts of an enterprise.

From a security perspective:

> Forests, Trees, and Domains define how trust is established, how identities are managed, and how attackers can move across an enterprise.

Understanding these concepts is essential for:

- Active Directory Security
- Enterprise Architecture
- Cross-Domain Authentication
- Privilege Escalation
- Lateral Movement
- Trust Relationship Analysis

---

# Active Directory Hierarchy

The hierarchy is organized as follows:

```
Forest

│

├── Tree

│     ├── Domain
│     ├── Domain
│     └── Domain

│

└── Tree

      ├── Domain
      └── Domain
```

Each level introduces a different administrative and security scope.

---

# Domain

A **Domain** is the fundamental administrative and security boundary within Active Directory.

A domain contains:

- Users
- Computers
- Groups
- Organizational Units (OUs)
- Group Policies
- Domain Controllers

Example:

```
corp.local
```

Each domain has:

- Its own Active Directory database
- Its own security policies
- Its own administrators
- One or more Domain Controllers

---

# Tree

A **Tree** is a collection of one or more domains that share a contiguous DNS namespace.

Example:

```
corp.local

├── hr.corp.local

├── finance.corp.local

└── dev.corp.local
```

Characteristics:

- Shared namespace
- Automatic trust relationships
- Common schema
- Common configuration

All domains inside the tree trust one another by default.

---

# Forest

A **Forest** is the highest logical and security boundary in Active Directory.

It contains:

- One or more Trees
- Shared schema
- Shared configuration
- Global Catalog
- Trust relationships

Example:

```
Forest

├── corp.local

│      ├── hr.corp.local

│      └── dev.corp.local

└── company.net

       └── sales.company.net
```

Domains inside different trees can have different DNS namespaces while still belonging to the same forest.

---

# Relationship Between Them

```
Forest

↓

Tree

↓

Domain

↓

Organizational Unit

↓

Objects
```

Each level adds organization while preserving centralized trust.

---

# Forest Root Domain

The first domain created becomes the:

```
Forest Root Domain
```

Responsibilities include:

- Forest administration
- Enterprise-wide configuration
- Enterprise Admins
- Schema Admins

The forest root domain is extremely sensitive because compromise often affects the entire forest.

---

# Trust Relationships

Trust is fundamental to Active Directory.

Default trusts include:

```
Child Domain

⇄

Parent Domain
```

```
Tree

⇄

Tree
```

```
All Domains

⇄

Forest
```

Authentication can flow across these trusted boundaries.

---

# Shared Components

All domains inside the same forest share:

| Component | Shared |
|------------|---------|
| Schema | Yes |
| Configuration | Yes |
| Global Catalog | Yes |
| Enterprise Admins | Yes |
| Trust Relationships | Yes |

However, each domain maintains its own:

- Users
- Groups
- Password policies
- Domain Controllers

---

# Global Catalog

The Global Catalog (GC) is available forest-wide.

It stores:

- Partial information about every object
- Universal Group memberships
- Searchable directory data

Used for:

- Enterprise-wide searches
- Cross-domain logons
- Object discovery

---

# Authentication Flow

Example:

```
User

↓

Child Domain

↓

Trust

↓

Parent Domain

↓

Resource Access
```

Trust relationships allow authentication across domains.

---

# Administrative Boundaries

### Domain Boundary

Controls:

- Local administration
- Domain policies
- User management

---

### Forest Boundary

Controls:

- Enterprise administration
- Schema modifications
- Trust relationships
- Global Catalog
- Enterprise-wide configuration

The forest is generally considered the ultimate security boundary.

---

# Security Importance

Understanding forests and trees helps explain:

- Cross-domain authentication
- Trust relationships
- Administrative delegation
- Enterprise privilege models
- Replication boundaries

---

# Offensive Perspective

Attackers analyze:

- Forest structure
- Child domains
- Trust relationships
- Enterprise Admins
- Cross-domain privileges
- Forest-wide attack paths

The objective is often to move from a compromised domain to the forest root.

---

# Common Attack Paths

| Target | Objective |
|----------|-----------|
| Child Domain | Initial compromise |
| Trust Relationships | Lateral movement |
| Enterprise Admins | Forest compromise |
| Global Catalog | Domain enumeration |
| Cross-domain ACLs | Privilege escalation |

---

# Common Enumeration Targets

Attackers identify:

- Forest name
- Domain names
- Tree hierarchy
- Domain Controllers
- Trusts
- Enterprise Admins
- Global Catalog servers

Understanding the directory structure enables efficient attack path mapping.

---

# Defensive Perspective

Administrators should:

- Minimize unnecessary trusts
- Restrict Enterprise Admin usage
- Audit cross-domain permissions
- Secure Forest Root Domain
- Monitor trust changes
- Limit administrative delegation
- Protect Global Catalog servers

The forest should be treated as the highest-value security asset.

---

# Domain vs Tree vs Forest

| Component | Scope | Purpose |
|------------|-------|---------|
| Domain | Administrative | Authentication & object management |
| Tree | Namespace | Organizes related domains |
| Forest | Security | Highest trust boundary |

---

# Example Enterprise

```
Forest

company.local

│

├── corp.company.local

├── hr.company.local

├── finance.company.local

└── research.company.local
```

All domains:

- Share the same forest
- Trust each other
- Share the schema
- Have separate administrators

---

# Why Forests, Trees, and Domains Matter

Understanding these structures allows security professionals to:

- Analyze trust relationships
- Understand authentication paths
- Identify lateral movement opportunities
- Assess enterprise-wide privileges
- Secure Active Directory environments

---

# Key Takeaways

- A **Domain** is the primary administrative and authentication boundary.
- A **Tree** groups related domains under a shared DNS namespace.
- A **Forest** is the highest security and trust boundary in Active Directory.
- Domains within a forest automatically establish trust relationships.
- The Forest Root Domain holds the highest administrative privileges.
- Compromising the forest often means compromising the entire enterprise.

---

# Key Insight

> In Active Directory, domains organize identities, trees organize namespaces, and forests organize trust. While administrators often manage domains independently, attackers think in terms of forests because the forest defines the ultimate security boundary of the enterprise.
