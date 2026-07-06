# Authentication vs Authorization (Security Perspective)

## Overview

Authentication and authorization are two fundamental security concepts that work together to protect applications and systems.

Although they are closely related, they solve different security problems.

From a security perspective:

> Authentication answers **"Who are you?"** while authorization answers **"What are you allowed to do?"**

Confusing these concepts is one of the most common causes of critical web application vulnerabilities.

---

# Authentication (Identity Verification)

## What is Authentication?

Authentication is the process of verifying the identity of a user, application, or service before granting access.

The objective is to prove that an entity is who it claims to be.

---

## Common Authentication Factors

Authentication is generally based on one or more of the following factors:

### Something You Know
- Password
- PIN
- Security questions

### Something You Have
- Smartphone
- Hardware token
- Smart card
- OTP application

### Something You Are
- Fingerprint
- Face recognition
- Iris scan

Using multiple factors results in **Multi-Factor Authentication (MFA)**.

---

## Common Authentication Methods

- Username and Password
- One-Time Password (OTP)
- Multi-Factor Authentication (MFA)
- OAuth
- OpenID Connect (OIDC)
- Kerberos
- Certificates
- Passkeys / WebAuthn

---

## Authentication Security Risks

Common authentication weaknesses include:

- Weak passwords
- Password reuse
- Brute-force attacks
- Credential stuffing
- Password spraying
- MFA fatigue attacks
- Session theft
- Poor password reset mechanisms
- Weak account recovery procedures

---

# Authorization (Access Control)

## What is Authorization?

Authorization determines which resources and actions an authenticated identity is allowed to access.

It always happens **after successful authentication**.

---

## Authorization Models

### Role-Based Access Control (RBAC)

Permissions are assigned to predefined roles.

Examples:

- User
- Manager
- Administrator

---

### Attribute-Based Access Control (ABAC)

Access decisions depend on attributes such as:

- Department
- Location
- Device
- Time
- Risk level

---

### Access Control Lists (ACL)

Permissions are assigned directly to individual users or groups.

---

### Policy-Based Access Control

Permissions are evaluated dynamically according to defined security policies.

---

# Authentication vs Authorization

| Authentication | Authorization |
|---------------|--------------|
| Verifies identity | Determines permissions |
| Happens first | Happens after authentication |
| "Who are you?" | "What can you do?" |
| Login process | Access decision |
| Creates identity | Enforces permissions |

---

# Common Authorization Vulnerabilities

Authorization failures are among the most critical web vulnerabilities.

Examples include:

## Broken Access Control

Users access resources outside their intended permissions.

---

## IDOR (Insecure Direct Object Reference)

Changing object identifiers to access another user's data.

Example:

```
GET /profile?id=1001
```

becomes

```
GET /profile?id=1002
```

---

## Privilege Escalation

A normal user gains administrator capabilities.

Types:

- Vertical Privilege Escalation
- Horizontal Privilege Escalation

---

## Missing Authorization Checks

Backend APIs trust the client without verifying permissions.

---

## Forced Browsing

Accessing hidden resources directly by guessing URLs.

---

# Authentication vs Session Management

Authentication and session management are different concepts.

Authentication verifies identity.

Session management maintains that authenticated identity across multiple requests.

Weak session management can completely bypass strong authentication.

---

# Pentester Perspective

During an assessment, authentication and authorization are tested separately.

Authentication Testing:

- Login functionality
- MFA implementation
- Password policies
- Account lockout
- Password reset
- Session creation

Authorization Testing:

- Role separation
- Object ownership validation
- API authorization
- Administrative endpoints
- Horizontal privilege escalation
- Vertical privilege escalation

---

# Real-World Examples

Authentication Failure:

- Stolen credentials
- Login bypass
- Weak MFA

Impact:

- Account compromise

---

Authorization Failure:

- Normal user accesses admin functionality
- User accesses another user's records

Impact:

- Sensitive data exposure
- Administrative compromise

---

# Attacker Perspective

Attackers first attempt to become authenticated.

Once authenticated, they focus on abusing authorization logic to expand their privileges.

Typical attacker workflow:

1. Obtain valid credentials
2. Establish authenticated session
3. Enumerate accessible resources
4. Test authorization boundaries
5. Escalate privileges
6. Access sensitive data
7. Maintain access

---

# Key Security Principles

Good authentication does **not** guarantee good authorization.

Every request should independently verify:

- Identity
- Permissions
- Resource ownership
- Intended action

Never trust information coming from the client.

---

# Key Insight

> Authentication establishes identity. Authorization enforces trust boundaries. In modern web applications, broken authorization is often more dangerous than broken authentication because attackers frequently obtain valid credentials before exploiting excessive permissions.
