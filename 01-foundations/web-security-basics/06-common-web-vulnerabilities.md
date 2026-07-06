# Common Web Vulnerabilities (Security Perspective)

## Overview

Web vulnerabilities are weaknesses in the design, implementation, or configuration of web applications that allow attackers to compromise confidentiality, integrity, or availability.

From a security perspective:

> Most web vulnerabilities exist because applications trust user-controlled input or fail to enforce security boundaries correctly.

---

# Why Web Vulnerabilities Matter

A single vulnerability can allow an attacker to:

- Access sensitive information
- Execute unauthorized actions
- Gain remote code execution
- Compromise user accounts
- Escalate privileges
- Pivot deeper into internal infrastructure

Modern attacks often combine multiple vulnerabilities rather than relying on a single flaw.

---

# Common Web Vulnerabilities

## 1. Injection

### Description

Injection occurs when untrusted input is interpreted as commands or queries by a backend component.

### Common Types

- SQL Injection
- OS Command Injection
- NoSQL Injection
- LDAP Injection
- XPath Injection

### Impact

- Database compromise
- Remote Code Execution (RCE)
- Sensitive data disclosure

---

## 2. Cross-Site Scripting (XSS)

### Description

An attacker injects malicious JavaScript that executes inside another user's browser.

### Common Types

- Reflected XSS
- Stored XSS
- DOM-based XSS

### Impact

- Session hijacking
- Cookie theft
- Account takeover
- Client-side phishing

---

## 3. Cross-Site Request Forgery (CSRF)

### Description

An authenticated user is tricked into performing unintended actions.

### Impact

- Password changes
- Financial transactions
- Administrative actions
- Account manipulation

---

## 4. Broken Access Control

### Description

The application fails to properly enforce authorization rules.

### Common Examples

- IDOR
- Missing role validation
- Forced browsing
- Privilege escalation

### Impact

- Unauthorized data access
- Administrative compromise
- Information disclosure

---

## 5. Authentication Vulnerabilities

### Description

Weak authentication mechanisms allow attackers to impersonate legitimate users.

### Common Examples

- Weak passwords
- Missing MFA
- Login bypass
- Credential stuffing

### Impact

- Account takeover
- Identity compromise

---

## 6. Session Management Vulnerabilities

### Description

Weak handling of authenticated sessions.

### Common Examples

- Session hijacking
- Session fixation
- Predictable session IDs
- Missing cookie protections

### Impact

- Authentication bypass
- Persistent unauthorized access

---

## 7. File Upload Vulnerabilities

### Description

Applications fail to properly validate uploaded files.

### Common Examples

- Executable file uploads
- MIME type bypass
- Extension bypass
- Path manipulation

### Impact

- Remote Code Execution
- Web shell deployment
- Server compromise

---

## 8. Server-Side Request Forgery (SSRF)

### Description

The server is manipulated into sending requests to unintended destinations.

### Common Targets

- Internal services
- Cloud metadata endpoints
- Localhost services
- Administrative interfaces

### Impact

- Internal network discovery
- Credential theft
- Cloud compromise

---

## 9. Directory Traversal (Path Traversal)

### Description

Improper file path validation allows access outside the intended directory.

### Common Targets

- Configuration files
- Password files
- Source code
- Backup files

### Impact

- Sensitive information disclosure
- Credential exposure
- Application compromise

---

## 10. Business Logic Vulnerabilities

### Description

Flaws in application workflows rather than technical implementation.

### Common Examples

- Price manipulation
- Coupon abuse
- Payment bypass
- Workflow manipulation
- Race conditions

### Impact

- Financial fraud
- Data inconsistency
- Unauthorized operations

---

# Additional High-Impact Vulnerabilities

Although not always listed separately, pentesters also evaluate:

- Insecure File Inclusion (LFI/RFI)
- Open Redirect
- XML External Entity (XXE)
- Clickjacking
- HTTP Request Smuggling
- Insecure Deserialization
- Prototype Pollution (JavaScript)
- API Authorization flaws

---

# Vulnerability Chaining

Real-world attacks rarely rely on a single vulnerability.

Example attack chain:

1. XSS steals a session cookie
2. Session hijacking authenticates the attacker
3. Broken Access Control exposes admin functionality
4. File Upload enables web shell execution
5. SSRF provides access to internal infrastructure

Attack chains are significantly more dangerous than isolated vulnerabilities.

---

# Pentester Perspective

A pentester evaluates each vulnerability by asking:

- Can user input become code execution?
- Can authentication be bypassed?
- Can authorization be abused?
- Can sensitive information be extracted?
- Can the application trust model be manipulated?

The objective is to understand how seemingly minor weaknesses can be chained into complete application compromise.

---

# Key Takeaways

- Most vulnerabilities originate from trusting unvalidated input.
- Broken Access Control consistently ranks among the most critical risks.
- Technical flaws and business logic flaws are equally important.
- Successful attacks typically combine multiple vulnerabilities.

---

# Key Insight

> Individual vulnerabilities rarely compromise an application alone the real danger comes from chaining multiple weaknesses together until trust boundaries collapse.
