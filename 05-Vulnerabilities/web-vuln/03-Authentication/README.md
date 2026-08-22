# Authentication Vulnerabilities

## Overview

Authentication vulnerabilities are security weaknesses that allow attackers to bypass, weaken, abuse, or compromise the mechanisms responsible for verifying a user's identity.

Authentication answers one fundamental security question:

> **Who are you?**

If this process is improperly designed or implemented, attackers may gain unauthorized access without exploiting memory corruption, injection flaws, or access control weaknesses.

Authentication vulnerabilities are among the highest-impact web security issues because successful exploitation often results in complete account compromise.

---

# Learning Path

1. Broken Authentication
2. Brute Force
3. Credential Stuffing
4. Password Spraying
5. Session Hijacking
6. Session Fixation
7. Session Prediction
8. JWT Attacks
9. OAuth Attacks
10. OpenID Connect (OIDC) Attacks
11. MFA Bypass
12. Password Reset Poisoning

---

# Core Concepts

This section covers attacks against the entire authentication ecosystem, including:

- Identity verification
- Password-based authentication
- Session management
- Authentication tokens
- Federated authentication
- Single Sign-On (SSO)
- Multi-Factor Authentication (MFA)
- Password recovery
- Token validation
- Authentication workflows

---

# Authentication Attack Surface

Authentication mechanisms commonly exposed to attackers include:

## Login Interfaces

- Username/password forms
- API authentication endpoints
- Mobile login flows

---

## Session Management

- Session identifiers
- Cookies
- Authentication tokens
- JWTs

---

## Federated Authentication

- OAuth 2.0
- OpenID Connect (OIDC)
- Single Sign-On (SSO)

---

## Account Recovery

- Password reset
- Recovery emails
- Backup codes
- Recovery tokens

---

## Multi-Factor Authentication

- One-Time Passwords (OTP)
- Push notifications
- Security keys
- Trusted devices

---

# Common Root Causes

Most authentication vulnerabilities arise from:

- Weak identity verification
- Poor session management
- Insecure token validation
- Weak password policies
- Improper authentication workflows
- Insecure account recovery
- Missing rate limiting
- Trusting client-controlled data
- Misconfigured authentication protocols

Authentication failures rarely stem from broken cryptography—they usually result from implementation flaws.

---

# Typical Attack Chain

Authentication attacks frequently follow this progression:

```
Authentication Weakness

↓

Credential or Token Compromise

↓

Authentication Bypass

↓

Authenticated Session

↓

Privilege Escalation

↓

Sensitive Data Access

↓

Application Compromise
```

Successful authentication compromise often serves as the initial access point for broader attacks.

---

# Defensive Principles

Secure authentication should include:

- Strong password policies
- Multi-Factor Authentication (MFA)
- Secure session management
- Cryptographically secure token generation
- Short-lived authentication tokens
- Rate limiting
- Account lockout
- Secure password recovery
- Proper OAuth and OIDC validation
- Continuous authentication monitoring

Authentication should be treated as a complete lifecycle rather than a single login event.

---

# Relationship to Other Vulnerability Categories

Authentication vulnerabilities often interact with other web security weaknesses.

| Category | Relationship |
|----------|--------------|
| Injection | May expose credentials or authentication tokens |
| Client-Side | XSS can steal sessions or JWTs |
| Authorization | Authentication establishes identity before authorization decisions |
| Business Logic | Authentication workflows may contain logic flaws |
| API Security | APIs rely heavily on secure token validation |

Compromising authentication frequently enables exploitation of additional vulnerability classes.

---

# Skills Developed

After completing this section, you should be able to:

- Analyze authentication architectures
- Assess session management security
- Evaluate JWT, OAuth, and OIDC implementations
- Test Multi-Factor Authentication
- Assess password recovery mechanisms
- Identify session-related weaknesses
- Evaluate authentication workflows
- Recommend secure authentication designs

---

# Next Section

Continue with:

**Authorization Vulnerabilities**

This section explores weaknesses in access control mechanisms, including:

- Broken Access Control
- IDOR / BOLA
- Privilege Escalation
- Mass Assignment
- Forced Browsing
- Authorization Logic Flaws

While authentication determines **who the user is**, authorization determines **what the user is allowed to do**.

---

> **Key Insight:** Authentication is the foundation of application security. Once an attacker successfully bypasses or compromises authentication, the application often treats them as a legitimate user, making secure identity verification, session management, and token validation critical to defending modern web applications.