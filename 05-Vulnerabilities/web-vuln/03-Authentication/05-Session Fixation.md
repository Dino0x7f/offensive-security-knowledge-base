# Session Fixation

## Overview

Session Fixation is a session management vulnerability in which an attacker forces or persuades a victim to use a session identifier that is already known to the attacker. If the application fails to generate a new session identifier after successful authentication, the attacker can later reuse the same session identifier to access the victim's authenticated session.

Unlike Session Hijacking, where an attacker steals an existing authenticated session, Session Fixation occurs **before authentication**. The attacker establishes the session first, waits for the victim to authenticate, and then reuses the fixed session identifier.

This vulnerability results from improper session lifecycle management rather than weak authentication.

---

# How Session Fixation Works

Typical attack flow:

```
Attacker Creates Session

↓

Known Session Identifier

↓

Victim Uses Same Session

↓

Victim Authenticates

↓

Application Keeps Session ID

↓

Attacker Reuses Session

↓

Authenticated Access
```

The application incorrectly associates the pre-existing session with the authenticated user.

---

# Root Cause

Session Fixation occurs because applications fail to issue a new session identifier after successful authentication.

Common causes include:

- Session identifier reuse
- Missing session regeneration
- Weak session lifecycle management
- Authentication without session renewal
- User-controlled session identifiers
- Predictable session handling
- Insecure session initialization

---

# Session Lifecycle

Secure session management should follow this model:

```
Anonymous Session

↓

Authentication

↓

Generate New Session ID

↓

Authenticated Session

↓

Logout

↓

Session Destroyed
```

If the session identifier remains unchanged after login, Session Fixation becomes possible.

---

# Attack Surface

Session Fixation commonly affects:

- Login pages
- Web portals
- Administrative interfaces
- Enterprise applications
- Legacy authentication systems
- Single Sign-On (SSO)
- Mobile web applications
- Stateful web frameworks

Applications using server-side sessions are particularly susceptible if they fail to regenerate session identifiers.

---

# Session Fixation Techniques

Attackers may attempt to fix session identifiers through:

- URL parameters
- Cookies
- Hidden form fields
- HTTP headers
- Browser manipulation
- Social engineering
- Session links

The exact method depends on how the application accepts or maintains session identifiers.

---

# Types of Session Fixation

## Cookie-Based Fixation

The attacker fixes the session identifier through browser cookies.

---

## URL-Based Fixation

The session identifier is embedded in a URL.

---

## Hidden Field Fixation

Session values are propagated through hidden HTML form fields.

---

## Header-Based Fixation

Applications accept session identifiers from HTTP headers.

---

# Potential Impact

Successful Session Fixation may allow attackers to:

- Bypass authentication
- Access authenticated user sessions
- Impersonate victims
- Access sensitive information
- Abuse authenticated functionality
- Escalate privileges
- Compromise administrator sessions
- Establish persistent access

The impact depends on the privileges acquired after the victim authenticates.

---

# Common Indicators

Possible indicators include:

- Session identifier unchanged after login
- User-controlled session identifiers
- Session reuse across authentication
- Long-lived anonymous sessions
- Missing session regeneration
- Authentication without new session creation

---

# Exploitation Requirements

Successful Session Fixation generally requires:

- Ability to predetermine the session identifier
- Victim authentication
- Session identifier persistence after login
- Lack of session regeneration

Without session reuse across authentication, Session Fixation is not possible.

---

# Mitigation

Recommended defenses include:

- Regenerate session identifiers after authentication
- Regenerate sessions after privilege changes
- Destroy anonymous sessions during login
- Use unpredictable session identifiers
- Prevent user-controlled session IDs
- Secure session cookies
- Short session lifetimes
- Invalidate sessions during logout

Applications should always establish a new authenticated session after successful login.

---

# Detection Methods

Security professionals identify Session Fixation through:

- Authentication workflow testing
- Session lifecycle analysis
- Cookie inspection
- Source code review
- Dynamic application testing
- Session identifier comparison before and after login

Testing focuses on verifying whether the session identifier changes after authentication.

---

# Session Fixation vs Session Hijacking

| Session Fixation | Session Hijacking |
|------------------|-------------------|
| Begins before authentication | Occurs after authentication |
| Attacker already knows the session identifier | Attacker steals an existing session identifier |
| Exploits missing session regeneration | Exploits session theft |
| Targets authentication workflow | Targets authenticated sessions |

Both attacks compromise authenticated sessions, but they exploit different stages of the session lifecycle.

---

# Relationship to Other Vulnerabilities

Session Fixation often serves as the initial stage of session compromise.

```
Known Session Identifier

↓

Victim Authentication

↓

Session Not Regenerated

↓

Authenticated Session Reused

↓

Privilege Abuse

↓

Application Compromise
```

Although less common in modern frameworks, legacy applications remain susceptible when session regeneration is omitted.

---

# Real-World Examples

Session Fixation vulnerabilities have historically affected:

- Legacy web applications
- Enterprise portals
- Java web applications
- PHP applications
- Administrative dashboards
- Internal business systems
- Custom authentication frameworks

Modern frameworks typically regenerate session identifiers automatically, reducing—but not eliminating—the risk.

---

# Importance in Offensive Security

Understanding Session Fixation enables penetration testers to:

- Assess session lifecycle security
- Evaluate authentication workflows
- Test session regeneration
- Analyze session management implementations
- Identify legacy session handling flaws
- Recommend secure session management practices

---

> **Key Insight:** Session Fixation succeeds because the application continues to trust a session established before authentication. Secure applications must always invalidate anonymous sessions and generate a completely new session identifier immediately after successful authentication.