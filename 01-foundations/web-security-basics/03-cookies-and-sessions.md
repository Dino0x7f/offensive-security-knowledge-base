# Cookies and Sessions (Security Perspective)

## Overview

HTTP is a stateless protocol, meaning each request is independent and the server does not automatically remember previous interactions.

To maintain user authentication and application state, web applications rely on **cookies** and **sessions**.

From a security perspective:

> Session management is one of the most critical components of web application security because possession of a valid session often equals possession of the user's identity.

---

# Cookies

## What is a Cookie?

A cookie is a small piece of data stored by the client's browser and automatically included in subsequent HTTP requests to the same domain.

Cookies may store:

- Session identifiers
- User preferences
- Authentication tokens
- Tracking information

Cookies are stored client-side and should never contain sensitive information in plaintext.

---

## Types of Cookies

### Session Cookies

- Stored in memory
- Deleted when the browser closes
- Commonly used for authenticated sessions

---

### Persistent Cookies

- Stored on disk
- Remain until expiration
- Used for "Remember Me" functionality

---

### First-Party Cookies

Created by the visited website.

---

### Third-Party Cookies

Created by external services (analytics, advertisements, tracking).

---

# Sessions

## What is a Session?

A session is a server-side mechanism used to maintain user state after successful authentication.

Instead of storing authentication data in the browser, the server stores session information internally and associates it with a unique **Session ID**.

---

## Typical Authentication Flow

1. User submits credentials
2. Server validates credentials
3. Server creates a session
4. Server generates a random Session ID
5. Session ID is returned as a cookie
6. Browser automatically includes the cookie in future requests
7. Server validates the Session ID on every request

---

## Session Lifecycle

A secure session should:

- Be generated using cryptographically secure randomness
- Expire after inactivity
- Expire after logout
- Be regenerated after authentication
- Be invalidated when privileges change

---

# Cookie Security Attributes

## Secure

Only sends the cookie over HTTPS connections.

Protects against network interception.

---

## HttpOnly

Prevents JavaScript from accessing cookies.

Mitigates cookie theft through XSS.

---

## SameSite

Controls whether cookies are sent with cross-site requests.

Values:

- Strict
- Lax
- None

Helps mitigate CSRF attacks.

---

## Domain

Specifies which domains may receive the cookie.

Improper configuration may expose cookies to subdomains.

---

## Path

Limits where the cookie is sent within the application.

Restricting the path reduces unnecessary exposure.

---

# Common Session Attacks

## 1. Session Hijacking

Attacker steals a valid session identifier.

Methods include:

- XSS
- MITM
- Malware
- Browser compromise

Impact:

- Complete account takeover

---

## 2. Session Fixation

Attacker forces the victim to authenticate using a known Session ID.

If the application fails to regenerate the session after login, the attacker can reuse it.

---

## 3. Cookie Theft

Cookies may be stolen through:

- Cross-Site Scripting (XSS)
- Browser malware
- Physical access
- Network interception (without HTTPS)

---

## 4. Session Prediction

Weak random number generation allows attackers to guess Session IDs.

Modern frameworks typically prevent this.

---

## 5. Session Replay

Previously captured session tokens are reused to impersonate users.

---

# Common Session Management Weaknesses

- Predictable Session IDs
- Missing Secure flag
- Missing HttpOnly flag
- Missing SameSite protection
- Sessions never expire
- Session not invalidated after logout
- Session ID not regenerated after login
- Session identifiers exposed in URLs

---

# Pentester Perspective

During web assessments, pentesters evaluate:

- Session generation quality
- Cookie attributes
- Session timeout behavior
- Logout functionality
- Session fixation resistance
- Session invalidation
- Cross-user session isolation
- Token entropy

Typical questions include:

- Can another user reuse this session?
- Can the Session ID be predicted?
- Does logout actually invalidate the session?
- Are cookies protected against XSS and CSRF?

---

# Defensive Best Practices

- Use HTTPS everywhere
- Enable Secure and HttpOnly
- Configure SameSite appropriately
- Regenerate Session IDs after authentication
- Implement idle and absolute session timeouts
- Invalidate sessions on logout
- Store only minimal information in cookies
- Use cryptographically secure random Session IDs

---

# Key Insight

> Authentication verifies identity once. Sessions preserve that identity. If an attacker obtains a valid session token, authentication becomes irrelevant.
