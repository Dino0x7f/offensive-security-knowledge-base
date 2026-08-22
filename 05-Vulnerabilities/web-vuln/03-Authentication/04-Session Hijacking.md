# Session Hijacking

## Overview

Session Hijacking is an authentication attack in which an attacker obtains or abuses a valid session identifier to impersonate an authenticated user without knowing the user's credentials.

After successful authentication, web applications typically issue a session token that represents the user's identity during subsequent requests. If an attacker acquires this token, the server treats the attacker as the legitimate user until the session expires or is invalidated.

Unlike password-based attacks, Session Hijacking targets the **authenticated session** rather than the login process itself.

---

# How Session Hijacking Works

Typical attack flow:

```
User Authenticates

↓

Session Created

↓

Session Identifier Issued

↓

Attacker Obtains Session Token

↓

Attacker Reuses Token

↓

Authenticated Access Granted
```

The server trusts the session identifier as proof of authentication.

---

# Root Cause

Session Hijacking occurs because session identifiers are exposed, predictable, improperly protected, or insufficiently validated.

Common causes include:

- Session token theft
- Weak session generation
- Predictable session identifiers
- Unencrypted communication
- Client-side token exposure
- Session reuse
- Missing session expiration
- Insecure storage of authentication tokens

---

# Session Lifecycle

Typical session lifecycle:

```
Authentication

↓

Session Generation

↓

Token Storage

↓

Authenticated Requests

↓

Session Expiration

↓

Logout
```

Weaknesses at any stage may enable session compromise.

---

# Attack Surface

Session Hijacking commonly targets:

- Session cookies
- Authentication tokens
- JWTs
- API access tokens
- Browser storage
- Mobile application sessions
- Single Sign-On (SSO)
- WebSocket authentication
- Persistent login mechanisms

---

# Types of Session Hijacking

## Cookie Theft

Attackers obtain session cookies and reuse them.

---

## Token Theft

Authentication tokens are stolen from storage or network traffic.

---

## Network Hijacking

Session identifiers are intercepted during transmission.

---

## Browser-Based Hijacking

Malicious client-side code accesses authentication tokens.

---

## Sidejacking

Session identifiers are captured over insecure wireless or network connections.

---

# Potential Impact

Successful Session Hijacking may allow attackers to:

- Access authenticated accounts
- Bypass authentication
- Perform actions as the victim
- Escalate privileges
- Access sensitive information
- Abuse authenticated APIs
- Maintain persistent access
- Compromise administrative sessions

The impact depends entirely on the privileges associated with the stolen session.

---

# Common Indicators

Possible indicators include:

- Simultaneous sessions from different locations
- Session reuse across devices
- Authentication without login events
- Geographic anomalies
- Unexpected session persistence
- Unusual authenticated activity

---

# Exploitation Requirements

Successful Session Hijacking generally requires:

- A valid session identifier
- Access to the session token
- A still-active session
- Weak session validation

No password is required if the session remains valid.

---

# Mitigation

Recommended defenses include:

- Secure session generation
- HTTPS for all authenticated traffic
- Secure cookie attributes
- HttpOnly cookies
- SameSite cookies
- Short session lifetimes
- Session rotation after authentication
- Session invalidation on logout
- Device and IP validation
- Continuous session monitoring

Applications should treat session identifiers as highly sensitive authentication credentials.

---

# Detection Methods

Security professionals identify Session Hijacking risks through:

- Session management testing
- Cookie security assessment
- Authentication workflow analysis
- Source code review
- Dynamic application testing
- Browser security inspection

Testing evaluates whether stolen or reused session identifiers remain valid under different conditions.

---

# Session Hijacking vs Session Fixation

| Session Hijacking | Session Fixation |
|-------------------|------------------|
| Steals an existing authenticated session | Forces the victim to use a predetermined session |
| Targets active sessions | Targets session creation |
| Attacker acquires a valid session token | Attacker already knows the session identifier |
| Occurs after authentication | Begins before authentication |

Although closely related, the two attacks target different stages of session management.

---

# Relationship to Other Vulnerabilities

Session Hijacking is often enabled by other vulnerabilities.

```
XSS / Network Exposure / Token Leakage

↓

Session Token Theft

↓

Session Hijacking

↓

Authenticated Access

↓

Privilege Abuse

↓

Application Compromise
```

Many authentication attacks ultimately aim to obtain a valid session rather than user credentials.

---

# Real-World Examples

Session Hijacking has affected:

- Banking platforms
- Enterprise web portals
- Cloud management consoles
- Administrative dashboards
- E-commerce websites
- Email services
- REST APIs
- Mobile applications

Historically, insecure session handling has been a leading cause of account compromise.

---

# Importance in Offensive Security

Understanding Session Hijacking enables penetration testers to:

- Assess session management security
- Evaluate cookie protection
- Test authentication token handling
- Analyze session lifecycle controls
- Assess logout mechanisms
- Recommend secure session management practices

---

> **Key Insight:** Session Hijacking bypasses authentication by stealing trust instead of credentials. Once a valid session identifier is obtained, the attacker inherits the victim's authenticated identity, making secure session management as critical as secure password handling.