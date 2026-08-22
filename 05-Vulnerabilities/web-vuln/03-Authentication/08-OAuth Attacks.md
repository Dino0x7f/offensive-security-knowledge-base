# OAuth Attacks

## Overview

OAuth Attacks are authentication and authorization attacks that exploit insecure implementations of the OAuth 2.0 authorization framework. Rather than targeting user passwords directly, these attacks abuse weaknesses in the authorization flow, token handling, client validation, or redirect mechanisms to gain unauthorized access to protected resources.

OAuth is an **authorization framework**, not an authentication protocol. However, many modern applications use OAuth together with OpenID Connect (OIDC) for user authentication, making implementation flaws highly impactful.

Most OAuth attacks arise from incorrect application design rather than weaknesses in the OAuth specification itself.

---

# OAuth Authorization Flow

Typical OAuth flow:

```
User

↓

Authorization Request

↓

Authorization Server

↓

User Consent

↓

Authorization Code

↓

Access Token

↓

Protected Resource
```

Each stage introduces security controls that must be correctly implemented.

---

# OAuth Components

OAuth involves several entities:

- Resource Owner (User)
- Client Application
- Authorization Server
- Resource Server
- Access Token
- Refresh Token

Security depends on validating interactions between all participants.

---

# Root Cause

OAuth attacks occur because applications improperly validate authorization requests, redirect URIs, access tokens, or client identities.

Common causes include:

- Weak redirect URI validation
- Missing `state` parameter validation
- Authorization code leakage
- Token exposure
- Client impersonation
- Insecure token storage
- Overly permissive scopes
- Weak client authentication

---

# Attack Surface

OAuth attacks commonly target:

- Single Sign-On (SSO)
- Mobile applications
- Cloud platforms
- REST APIs
- OAuth login integrations
- Social login systems
- Enterprise identity providers
- Third-party application integrations

Applications integrating external identity providers are especially attractive targets.

---

# Common OAuth Attacks

## Authorization Code Interception

Attackers obtain a valid authorization code before it is exchanged.

---

## Access Token Theft

Valid access tokens are stolen from browsers, storage, or network traffic.

---

## Redirect URI Manipulation

Weak redirect URI validation allows authorization codes or tokens to be sent to attacker-controlled endpoints.

---

## CSRF Against OAuth

Missing or improperly validated `state` parameters enable request forgery during the authorization flow.

---

## Client Impersonation

Attackers impersonate legitimate OAuth clients to obtain valid tokens.

---

## Scope Abuse

Applications grant excessive permissions beyond what is required.

---

## Refresh Token Theft

Attackers obtain long-lived refresh tokens and generate new access tokens.

---

## Token Replay

Previously issued tokens are reused without proper validation or revocation.

---

# Potential Impact

Successful OAuth attacks may allow attackers to:

- Bypass authentication
- Hijack user accounts
- Steal access tokens
- Access protected APIs
- Escalate privileges
- Abuse third-party integrations
- Maintain persistent access
- Compromise cloud resources

The impact depends on the permissions associated with the compromised tokens.

---

# Common Indicators

Possible indicators include:

- Unexpected redirect destinations
- Missing `state` validation
- Weak client authentication
- Token leakage
- Long-lived access tokens
- Excessive OAuth scopes
- Reusable authorization codes

---

# Exploitation Requirements

Successful exploitation generally requires:

- Weak OAuth implementation
- Improper redirect validation
- Insecure token handling
- Insufficient client verification

Correctly implemented OAuth significantly reduces the likelihood of successful attacks.

---

# Mitigation

Recommended defenses include:

- Strict redirect URI validation
- Mandatory `state` parameter validation
- Use Proof Key for Code Exchange (PKCE)
- Short-lived access tokens
- Secure refresh token storage
- Scope minimization
- Token revocation support
- Secure client authentication
- HTTPS for all OAuth communication
- Continuous token monitoring

Applications should follow the latest OAuth 2.0 Security Best Current Practice recommendations.

---

# Detection Methods

Security professionals identify OAuth vulnerabilities through:

- Authorization flow testing
- Redirect URI validation
- Token manipulation
- Source code review
- API security assessment
- Dynamic application testing

Testing evaluates every stage of the OAuth authorization process.

---

# OAuth vs OpenID Connect

| OAuth 2.0 | OpenID Connect (OIDC) |
|------------|-----------------------|
| Authorization framework | Authentication layer built on OAuth |
| Grants access to resources | Verifies user identity |
| Uses Access Tokens | Uses ID Tokens in addition to Access Tokens |
| Answers "What can this client access?" | Answers "Who is this user?" |

Many modern applications use both technologies together.

---

# Relationship to Other Vulnerabilities

OAuth attacks often enable broader authentication compromise.

```
OAuth Misconfiguration

↓

Authorization Flow Abuse

↓

Token Theft

↓

Authenticated Access

↓

Privilege Escalation

↓

Application Compromise
```

OAuth vulnerabilities are frequently combined with CSRF, Open Redirect, Session Hijacking, or Broken Access Control.

---

# Real-World Examples

OAuth implementation flaws have affected:

- Social login providers
- Cloud platforms
- Enterprise identity systems
- Mobile applications
- API gateways
- Single Sign-On solutions
- SaaS platforms
- Third-party integrations

Most real-world incidents stem from insecure implementation rather than flaws in the OAuth protocol itself.

---

# Importance in Offensive Security

Understanding OAuth attacks enables penetration testers to:

- Assess authorization workflows
- Evaluate token security
- Test redirect URI validation
- Analyze client authentication
- Assess OAuth integrations
- Recommend secure OAuth implementations

---

> **Key Insight:** OAuth is designed to delegate authorization securely, but its security depends entirely on correct implementation. Weak redirect validation, improper token handling, or missing request verification can allow attackers to obtain valid authorization without ever compromising a user's password.