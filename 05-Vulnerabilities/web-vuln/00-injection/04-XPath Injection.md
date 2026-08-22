# XPath Injection

## Overview

XPath Injection is a vulnerability that occurs when untrusted user input is incorporated into an XPath expression without proper validation or escaping. An attacker can manipulate the XPath query to alter its intended logic, allowing unauthorized access to XML data, authentication bypass, or disclosure of sensitive information.

XPath Injection is conceptually similar to SQL Injection and LDAP Injection. However, instead of targeting relational databases or directory services, it targets **XML documents** queried using the **XPath language**.

Although XML-based applications are less common today than JSON-based applications, XPath Injection remains relevant in enterprise software, legacy systems, SOAP services, identity providers, and XML processing applications.

---

# What is XPath?

XPath (XML Path Language) is a query language used to navigate and retrieve data from XML documents.

Example XML:

```xml
<users>
    <user>
        <username>alice</username>
        <password>secret</password>
    </user>
</users>
```

Example XPath query:

```xpath
/users/user[username='alice']
```

This selects the user whose username is **alice**.

---

# How XPath Injection Works

Typical flow:

```
User Input

↓

Application

↓

XPath Expression

↓

XML Document

↓

Application Response
```

If user input becomes part of the XPath expression, attackers may manipulate the query logic.

---

## Example

Application builds:

```xpath
/users/user[
    username='<username>'
    and
    password='<password>'
]
```

Normal input:

```
alice

secret
```

Generated query:

```xpath
/users/user[
    username='alice'
    and
    password='secret'
]
```

If the application directly inserts user input into the expression, attackers may alter the logical conditions.

---

# Root Cause

XPath Injection occurs because applications fail to separate user-controlled data from XPath syntax.

Common causes include:

- Dynamic XPath construction
- Missing input escaping
- Unsafe string concatenation
- Improper XML processing
- Insecure authentication implementation

---

# Attack Surface

XPath Injection commonly appears in:

- Login forms
- XML-backed web applications
- SOAP services
- XML configuration systems
- Identity providers
- Legacy enterprise applications
- XML search functionality

---

# Common Targets

XPath Injection affects applications using:

- XML databases
- SOAP Web Services
- XML authentication systems
- SAML-based applications
- Enterprise XML repositories
- Configuration management systems

---

# XPath Operators

XPath expressions support logical operators.

Common examples include:

| Operator | Description |
|----------|-------------|
| `and` | Logical AND |
| `or` | Logical OR |
| `=` | Equality |
| `!=` | Inequality |
| `contains()` | String matching |
| `starts-with()` | Prefix matching |
| `position()` | Node position |
| `last()` | Last node |

Improper handling of these operators may allow attackers to manipulate query logic.

---

# Types of XPath Injection

## Authentication Bypass

Attackers manipulate login queries to bypass authentication.

---

## XML Data Disclosure

Injected queries return unauthorized XML nodes.

Possible information includes:

- User accounts
- Password hashes
- Email addresses
- Configuration values
- API keys
- Secrets

---

## XML Structure Enumeration

Attackers infer the structure of the XML document.

They may enumerate:

- Node names
- Attributes
- Child elements
- Document hierarchy

---

## Blind XPath Injection

Applications do not directly return XML results.

Attackers infer information through:

- Boolean responses
- Application behavior
- Response timing
- Error conditions

---

# Potential Impact

Successful XPath Injection may allow attackers to:

- Bypass authentication
- Read confidential XML data
- Enumerate application structure
- Access configuration information
- Extract credentials
- Discover sensitive business data
- Assist further attacks

---

# Common Indicators

Possible indicators include:

- Authentication bypass
- XML parsing errors
- Unexpected search results
- Different application responses
- XML-related exceptions
- Enumeration of XML content

---

# Mitigation

Recommended defenses include:

- Parameterized XPath APIs
- Proper input escaping
- Allow-list input validation
- Secure XML processing
- Avoid dynamic XPath construction
- Least-privileged application design
- Secure authentication mechanisms

Applications should never concatenate user input directly into XPath expressions.

---

# Detection Methods

Security professionals identify XPath Injection through:

- Manual testing
- Source code review
- XML request analysis
- Dynamic security testing
- Fuzzing
- Automated vulnerability scanners

---

# XPath Injection vs SQL Injection

| SQL Injection | XPath Injection |
|--------------|-----------------|
| Targets relational databases | Targets XML documents |
| Uses SQL syntax | Uses XPath expressions |
| Database manipulation | XML query manipulation |
| SQL parser executes query | XPath engine evaluates expression |

---

# Relationship to Other Injection Vulnerabilities

XPath Injection belongs to the broader family of injection vulnerabilities.

```
Injection

├── SQL Injection
├── NoSQL Injection
├── LDAP Injection
├── XPath Injection
├── Command Injection
└── Template Injection
```

Although the target technologies differ, the underlying weakness remains the same: **user-controlled input is interpreted as executable query logic**.

---

# Real-World Examples

XPath Injection has been identified in:

- Legacy enterprise portals
- XML authentication systems
- SOAP APIs
- SAML identity providers
- XML configuration interfaces
- Document management systems

While less common than SQL Injection, XPath Injection can still expose highly sensitive enterprise data.

---

# Importance in Offensive Security

Understanding XPath Injection enables penetration testers to:

- Assess XML-based applications
- Evaluate authentication systems
- Test SOAP services
- Analyze XML processing logic
- Identify insecure query construction
- Recommend secure XML handling practices

---

## Prerequisites

Before studying XPath Injection, you should understand:

- XML Fundamentals
- XPath Basics
- SOAP Web Services
- SQL Injection
- LDAP Injection
- Web Fundamentals

---

> **Key Insight:** XPath Injection occurs when applications treat untrusted input as part of an XPath expression rather than as data. By manipulating query logic, attackers may bypass authentication, enumerate XML structures, or retrieve sensitive information from XML-backed systems.