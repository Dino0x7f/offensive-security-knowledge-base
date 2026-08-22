# LDAP Injection

## Overview

LDAP Injection is a vulnerability that occurs when untrusted user input is incorporated into LDAP (Lightweight Directory Access Protocol) queries without proper validation or escaping. An attacker can manipulate the structure of an LDAP query to alter its intended logic, potentially bypassing authentication, enumerating directory objects, or accessing sensitive directory information.

LDAP Injection is conceptually similar to SQL Injection. However, instead of targeting relational databases, it targets directory services such as Microsoft Active Directory or OpenLDAP.

Because enterprise authentication often relies on LDAP, successful exploitation may compromise the identity infrastructure of an organization.

---

# What is LDAP?

LDAP (Lightweight Directory Access Protocol) is a protocol used to query and manage directory services.

Common directory services include:

- Microsoft Active Directory
- OpenLDAP
- Apache Directory
- Oracle Internet Directory
- IBM Security Directory Server

LDAP stores structured identity information such as:

- Users
- Groups
- Organizational Units (OUs)
- Computers
- Printers
- Certificates

---

# LDAP Query Structure

LDAP queries use filters instead of SQL syntax.

Example:

```ldap
(uid=john)
```

Search for a specific user:

```ldap
(&(uid=john)(userPassword=password))
```

Meaning:

```
Find user

AND

Password matches
```

---

# How LDAP Injection Works

Typical authentication flow:

```
User Input

↓

Application

↓

LDAP Filter

↓

Directory Server

↓

Authentication Result
```

If user input is concatenated directly into the LDAP filter, an attacker may manipulate the query logic.

---

## Example

Application builds:

```ldap
(&(uid=<username>)(userPassword=<password>))
```

Normal input:

```
john

password123
```

Generated query:

```ldap
(&(uid=john)(userPassword=password123))
```

If user input modifies the filter structure, the authentication logic may change.

---

# Root Cause

LDAP Injection occurs because applications fail to separate user-controlled data from LDAP filter syntax.

Common causes include:

- Dynamic LDAP filter construction
- Missing input escaping
- Improper input validation
- Unsafe authentication routines
- Custom LDAP query builders

---

# Attack Surface

LDAP Injection commonly appears in:

- Login pages
- Single Sign-On portals
- Active Directory authentication
- Employee portals
- VPN authentication
- Identity management systems
- Enterprise web applications

---

# Common Targets

LDAP Injection primarily affects:

- Microsoft Active Directory
- OpenLDAP
- Enterprise Authentication Systems
- Identity Providers
- Internal Administrative Portals

---

# LDAP Special Characters

LDAP filters reserve several characters for query logic.

| Character | Meaning |
|-----------|---------|
| `*` | Wildcard |
| `(` | Start filter |
| `)` | End filter |
| `&` | Logical AND |
| `|` | Logical OR |
| `!` | Logical NOT |
| `\` | Escape character |

Improper handling of these characters may allow attackers to manipulate queries.

---

# Types of LDAP Injection

## Authentication Bypass

Attackers alter authentication filters to bypass login checks.

---

## Information Disclosure

Manipulated filters return additional directory objects.

Possible information includes:

- Usernames
- Email addresses
- Group memberships
- Organizational Units
- Computer accounts

---

## Directory Enumeration

Attackers systematically query the directory to map:

- Users
- Groups
- Domain structure
- Organizational hierarchy

---

## Privilege Escalation

Poorly implemented LDAP authorization logic may allow attackers to gain elevated access.

---

# Potential Impact

Successful LDAP Injection may allow attackers to:

- Bypass authentication
- Enumerate users
- Retrieve sensitive directory information
- Discover administrative accounts
- Enumerate group memberships
- Map Active Directory structure
- Assist credential attacks
- Support lateral movement

---

# Common Indicators

Possible indicators include:

- Unexpected authentication behavior
- Abnormal LDAP queries
- Directory enumeration
- Large authentication requests
- Repeated LDAP search operations
- LDAP server error messages

---

# Mitigation

Recommended defenses include:

- Parameterized LDAP APIs
- Proper LDAP escaping
- Strict input validation
- Allow-list validation
- Least-privileged LDAP accounts
- Secure authentication design
- Avoid dynamic filter construction

Applications should never concatenate user input directly into LDAP filters.

---

# Detection Methods

Security professionals identify LDAP Injection using:

- Manual testing
- Source code review
- Authentication testing
- Dynamic security testing
- Directory traffic analysis
- Automated vulnerability scanners

---

# SQL Injection vs LDAP Injection

| SQL Injection | LDAP Injection |
|--------------|----------------|
| Targets SQL databases | Targets LDAP directories |
| Manipulates SQL statements | Manipulates LDAP filters |
| Uses SQL syntax | Uses LDAP filter syntax |
| Database compromise | Directory compromise |

---

# Relationship to Active Directory

LDAP Injection is especially relevant in Active Directory environments.

Possible attack chain:

```
LDAP Injection

↓

Directory Enumeration

↓

User Discovery

↓

Privilege Mapping

↓

Password Attack

↓

Domain Compromise
```

Although LDAP Injection alone rarely compromises an entire domain, it often provides valuable reconnaissance information for later attacks.

---

# Real-World Examples

LDAP Injection has been observed in:

- Enterprise login portals
- HR systems
- VPN gateways
- Identity management platforms
- Internal web applications
- Active Directory-integrated services

---

# Importance in Offensive Security

Understanding LDAP Injection enables penetration testers to:

- Assess enterprise authentication systems
- Evaluate directory security
- Test Active Directory integrations
- Identify insecure LDAP queries
- Demonstrate authentication weaknesses
- Recommend secure directory access practices

---

## Prerequisites

Before studying LDAP Injection, you should understand:

- Active Directory Fundamentals
- LDAP Basics
- Authentication Mechanisms
- Web Fundamentals
- SQL Injection
- NoSQL Injection

---

## Next Step

Continue with:

**04-XPath Injection.md**

This chapter explores XPath Injection, where attackers manipulate XML query expressions to bypass authentication, retrieve unauthorized XML data, or alter application logic.

---

> **Key Insight:** LDAP Injection is fundamentally an injection vulnerability against directory services. Whenever applications build LDAP filters using untrusted user input without proper escaping, attackers may manipulate directory queries to bypass authentication, enumerate identities, or access sensitive directory information.