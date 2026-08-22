# Broken Authentication

## Overview

Broken Authentication is a class of vulnerabilities that occurs when an application improperly implements identity verification, session management, or authentication workflows, allowing attackers to impersonate legitimate users or bypass authentication controls.

Authentication is responsible for verifying **who a user is** before granting access to protected resources. Weaknesses in this process can result in unauthorized account access, credential compromise, session hijacking, or complete application takeover.

Broken Authentication is considered one of the most critical web security risks because compromising authentication often provides direct access to sensitive functionality without requiring further exploitation.

---

# Authentication Process

A typical authentication workflow consists of:

```
User

↓

Credentials Submitted

↓

Identity Verification

↓

Authentication Decision

↓

Session Creation

↓

Access Granted
```

If any stage is improperly implemented, attackers may compromise the authentication process.

---

# Root Cause

Broken Authentication occurs when applications fail to securely verify user identity or properly manage authenticated sessions.

Common causes include:

- Weak authentication logic
- Poor session management
- Insecure password handling
- Predictable authentication tokens
- Missing rate limiting
- Improper logout mechanisms
- Insecure password recovery
- Weak multi-factor authentication implementation

---

# Authentication Components

Authentication involves multiple security mechanisms:

### Identity Verification

Verifies the user's claimed identity.

---

### Credential Validation

Checks passwords, tokens, certificates, or other authentication factors.

---

### Session Management

Maintains authenticated user state.

---

### Authentication Tokens

Represents authenticated users during subsequent requests.

---

### Multi-Factor Authentication (MFA)

Adds additional identity verification beyond passwords.

Weaknesses in any component may compromise authentication.

---

# Attack Surface

Broken Authentication commonly appears in:

- Login pages
- Session management
- Password reset functionality
- Registration systems
- Remember-me features
- Single Sign-On (SSO)
- OAuth implementations
- Multi-Factor Authentication
- API authentication
- Mobile authentication

---

# Common Authentication Weaknesses

Broken Authentication may include:

- Weak password policies
- Predictable session identifiers
- Session fixation
- Session hijacking
- Missing account lockout
- Weak password reset mechanisms
- Improper logout
- Credential reuse
- Authentication bypass
- Insecure token generation

Each weakness increases the likelihood of unauthorized access.

---

# Potential Impact

Successful exploitation may allow attackers to:

- Access user accounts
- Hijack active sessions
- Bypass authentication
- Escalate privileges
- Access sensitive information
- Impersonate administrators
- Compromise business processes
- Achieve complete application takeover

Because authentication protects every protected resource, its compromise often has severe consequences.

---

# Common Indicators

Possible indicators include:

- Predictable session behavior
- Unlimited login attempts
- Weak password requirements
- Missing session expiration
- Password reset weaknesses
- Authentication logic inconsistencies
- Reusable authentication tokens

---

# Mitigation

Recommended defenses include:

- Strong password policies
- Secure password hashing
- Multi-Factor Authentication (MFA)
- Secure session identifiers
- Session expiration
- Account lockout mechanisms
- Rate limiting
- Secure password reset workflows
- Secure token generation
- Continuous authentication monitoring

Authentication should verify both user identity and request legitimacy throughout the session lifecycle.

---

# Detection Methods

Security professionals identify Broken Authentication through:

- Manual authentication testing
- Session analysis
- Source code review
- Dynamic Application Security Testing (DAST)
- Authentication workflow assessment
- Automated vulnerability scanners

Testing should evaluate the entire authentication lifecycle rather than only the login page.

---

# Broken Authentication vs Broken Access Control

| Broken Authentication | Broken Access Control |
|------------------------|----------------------|
| Verifies user identity | Determines user permissions |
| Answers "Who are you?" | Answers "What can you access?" |
| Authentication failure | Authorization failure |
| Leads to account compromise | Leads to privilege abuse |

Authentication and authorization are distinct security mechanisms that complement one another.

---

# Relationship to Other Vulnerabilities

Broken Authentication often enables broader attack chains.

```
Authentication Weakness

↓

Unauthorized Login

↓

Session Compromise

↓

Privilege Escalation

↓

Sensitive Data Access

↓

Application Compromise
```

It is frequently combined with session management flaws, credential attacks, or authorization weaknesses.

---

# Real-World Examples

Broken Authentication vulnerabilities have affected:

- Banking platforms
- Cloud management systems
- Enterprise applications
- E-commerce websites
- Social networking platforms
- Mobile applications
- REST APIs
- Administrative portals

Many major security incidents have originated from weaknesses in authentication rather than software vulnerabilities.

---

# Importance in Offensive Security

Understanding Broken Authentication enables penetration testers to:

- Assess authentication workflows
- Evaluate session security
- Test identity verification mechanisms
- Analyze password management
- Assess authentication token security
- Recommend secure authentication architecture

---

> **Key Insight:** Authentication establishes trust between users and applications. When authentication mechanisms fail, attackers no longer need to exploit business logic or software flaws they simply become legitimate users from the application's perspective, making Broken Authentication one of the most critical classes of web security vulnerabilities.