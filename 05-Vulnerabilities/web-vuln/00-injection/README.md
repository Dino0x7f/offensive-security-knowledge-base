# Injection Vulnerabilities

## Overview

Injection vulnerabilities occur when an application interprets untrusted user input as executable instructions instead of ordinary data. Rather than treating external input as passive information, the application passes it to an interpreter, parser, template engine, operating system, or protocol processor where it alters the intended execution flow.

Injection remains one of the oldest, most prevalent, and most impactful classes of security vulnerabilities. It affects virtually every layer of modern applications—from databases and operating systems to XML parsers, template engines, directory services, spreadsheets, and web protocols.

Understanding injection vulnerabilities is fundamental for penetration testing, secure software development, exploit development, and defensive security.

---

## Learning Path

1. SQL Injection
2. NoSQL Injection
3. Command Injection
4. LDAP Injection
5. XPath Injection
6. XML Injection
7. XML External Entity (XXE)
8. Server-Side Template Injection (SSTI)
9. Expression Language Injection
10. CRLF Injection
11. HTTP Header Injection
12. CSV Injection (Formula Injection)

---

## Focus Areas

This section focuses on:

- Injection fundamentals
- Interpreter abuse
- Query manipulation
- Command execution
- Template evaluation
- XML processing
- Directory service attacks
- Protocol manipulation
- Server-side execution
- Client-side formula execution

---

## Injection Targets

Injection vulnerabilities can target multiple execution environments.

### Database Engines

- SQL
- NoSQL

### Operating Systems

- Shell Commands
- Process Execution

### Directory Services

- LDAP

### XML Technologies

- XML Documents
- XPath
- XML Parsers (XXE)

### Server Frameworks

- Template Engines
- Expression Languages

### Network Protocols

- HTTP Headers
- CRLF Sequences

### Client Applications

- Spreadsheet Formula Engines

Each interpreter has its own syntax, execution model, and security considerations, but they all share the same fundamental weakness: **untrusted input is interpreted as executable logic.**

---

## Common Root Causes

Most injection vulnerabilities arise from one or more of the following:

- Dynamic query construction
- String concatenation
- Missing parameterization
- Improper input validation
- Unsafe parser configuration
- Insecure template rendering
- Trusting client-controlled data
- Mixing commands with data

---

## Security Perspective

Injection vulnerabilities are not limited to a specific technology or language. They represent a failure to enforce the boundary between **data** and **instructions**.

From a security perspective, every interpreter introduces a potential trust boundary:

```
User Input

↓

Application

↓

Interpreter

↓

Execution
```

Whenever user-controlled data crosses this boundary without proper separation, injection becomes possible.

---

## Attack Progression

Injection vulnerabilities often serve as the initial stage of larger attack chains.

```
User Input

↓

Injection Vulnerability

↓

Interpreter Manipulation

↓

Information Disclosure

↓

Authentication Bypass

↓

Privilege Escalation

↓

Remote Code Execution

↓

System Compromise
```

Not every injection vulnerability results in full system compromise, but many provide attackers with a powerful foothold.

---

## Defensive Principles

Regardless of the underlying technology, effective defenses follow the same principles:

- Separate code from data
- Use parameterized APIs
- Avoid dynamic command construction
- Validate user input
- Encode output appropriately
- Disable unnecessary interpreter features
- Apply least privilege
- Keep frameworks and libraries updated
- Perform regular security testing

---

## Next Step

After completing **Injection Vulnerabilities**, continue with other web vulnerability categories such as:

- Authentication Vulnerabilities
- Authorization Vulnerabilities
- File Handling Vulnerabilities
- Server-Side Vulnerabilities
- Client-Side Vulnerabilities
- Business Logic Vulnerabilities

Together, these topics provide a comprehensive understanding of modern web application security.

---

> **Key Insight:** Every injection vulnerability is ultimately the same design failure expressed through a different interpreter. Whether targeting SQL, LDAP, XML, operating system commands, template engines, or HTTP headers, the core issue remains unchanged: the application allows untrusted input to be executed as instructions instead of treating it as data.