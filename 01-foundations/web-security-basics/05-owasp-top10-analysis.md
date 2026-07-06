# OWASP Top 10 (Security Perspective)

## Overview

The OWASP Top 10 is a community-driven awareness document that highlights the most critical security risks affecting modern web applications.

From a security perspective:

> The OWASP Top 10 is not a vulnerability database—it is a representation of the attack patterns most frequently observed in real-world applications.

---

# Why OWASP Matters

The OWASP Top 10 helps:

- Prioritize security testing
- Understand common attack vectors
- Improve secure development practices
- Reduce application attack surface
- Guide penetration testing methodologies

For pentesters, it serves as a roadmap for identifying high-impact weaknesses.

---

# OWASP Top 10 Categories (2021)

## A01: Broken Access Control

### Description

The application fails to properly enforce authorization rules.

Attackers may access resources or perform actions beyond their intended privileges.

### Common Examples

- IDOR (Insecure Direct Object References)
- Missing authorization checks
- Privilege escalation
- Forced browsing
- Insecure API authorization

### Impact

- Unauthorized data access
- Administrative takeover
- Data modification
- Complete application compromise

---

## A02: Cryptographic Failures

### Description

Sensitive information is exposed due to weak or improper cryptographic implementation.

### Common Examples

- Weak password hashing
- Missing encryption
- Weak TLS configuration
- Hardcoded secrets
- Poor key management

### Impact

- Credential exposure
- Sensitive data leakage
- Compliance violations

---

## A03: Injection

### Description

Untrusted user input is interpreted as commands or queries.

### Common Examples

- SQL Injection
- OS Command Injection
- LDAP Injection
- NoSQL Injection
- XPath Injection

### Impact

- Database compromise
- Remote command execution
- Full system compromise

---

## A04: Insecure Design

### Description

Security problems introduced during system architecture or business logic design.

### Common Examples

- Missing authorization workflows
- Insecure password reset logic
- Business logic abuse
- Lack of rate limiting

### Impact

- Authentication bypass
- Logic manipulation
- Fraud scenarios

---

## A05: Security Misconfiguration

### Description

Unsafe or incorrect system configuration exposes unnecessary attack surface.

### Common Examples

- Debug mode enabled
- Default credentials
- Directory listing
- Verbose error messages
- Misconfigured cloud services

### Impact

- Information disclosure
- Initial compromise
- Privilege escalation

---

## A06: Vulnerable and Outdated Components

### Description

Applications rely on software containing publicly known vulnerabilities.

### Common Examples

- Outdated frameworks
- Vulnerable libraries
- Unsupported software versions

### Impact

- Remote Code Execution (RCE)
- Data breaches
- Supply chain compromise

---

## A07: Identification and Authentication Failures

### Description

Weak authentication mechanisms allow attackers to impersonate legitimate users.

### Common Examples

- Weak passwords
- Credential stuffing
- Session management flaws
- Missing MFA

### Impact

- Account takeover
- Privilege abuse
- Persistent unauthorized access

---

## A08: Software and Data Integrity Failures

### Description

Applications fail to verify the integrity of software, updates, or trusted data.

### Common Examples

- Insecure CI/CD pipelines
- Unsigned software updates
- Dependency confusion
- Insecure deserialization

### Impact

- Supply chain attacks
- Arbitrary code execution
- Malware distribution

---

## A09: Security Logging and Monitoring Failures

### Description

Security events are not properly recorded, monitored, or investigated.

### Common Examples

- Missing audit logs
- Disabled monitoring
- Insufficient alerting
- Log tampering

### Impact

- Delayed incident response
- Undetected compromise
- Difficult forensic investigation

---

## A10: Server-Side Request Forgery (SSRF)

### Description

The application allows attackers to force the server to make unintended requests.

### Common Targets

- Internal services
- Cloud metadata endpoints
- Administrative interfaces
- Internal APIs

### Impact

- Internal network discovery
- Cloud credential theft
- Privilege escalation
- Remote service abuse

---

# Pentester Perspective

Rather than memorizing the categories, pentesters think in terms of attack objectives:

- Can input become code execution?
- Can authentication be bypassed?
- Can authorization be abused?
- Can sensitive data be extracted?
- Can trust relationships be manipulated?

OWASP categories simply organize these attack patterns.

---

# Relationship Between Categories

Many successful attacks combine multiple OWASP risks.

Example attack chain:

1. Security Misconfiguration exposes a debug endpoint
2. Injection provides code execution
3. Broken Access Control grants administrative privileges
4. Logging Failures delay detection
5. Cryptographic Failures expose sensitive data

Real-world compromises rarely involve a single vulnerability.

---

# Key Takeaways

- OWASP Top 10 is a risk awareness framework, not an exhaustive list.
- Most critical breaches involve multiple chained weaknesses.
- Understanding the categories helps prioritize testing and remediation.
- Pentesters use OWASP as a guide—not a checklist.

---

# Key Insight

> The OWASP Top 10 does not teach individual exploits it teaches where modern web applications fail most often, allowing attackers to chain small weaknesses into complete compromise.
> 
