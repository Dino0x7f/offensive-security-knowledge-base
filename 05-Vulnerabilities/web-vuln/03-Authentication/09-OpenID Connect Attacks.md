# OpenID Connect (OIDC) Attacks

## Overview

OpenID Connect (OIDC) Attacks are authentication attacks that exploit insecure implementations of the OpenID Connect protocol to impersonate users, bypass authentication, steal identity tokens, or compromise trust relationships between applications and identity providers.

OpenID Connect is an **authentication protocol** built on top of OAuth 2.0. While OAuth is responsible for delegated authorization, OIDC introduces user authentication through the use of **ID Tokens** and standardized identity claims.

Most OIDC attacks target implementation flaws such as improper ID Token validation, insecure client configuration, weak redirect handling, or trust misconfigurations rather than weaknesses in the OIDC specification itself.

---

# OIDC Authentication Flow

Typical authentication process:

```
User

↓

Client Application

↓

Identity Provider (IdP)

↓

User Authentication

↓

Authorization Code

↓

ID Token + Access Token

↓

User Authenticated
```

Security depends on correctly validating every token and identity claim received from the Identity Provider.

---

# OIDC Components

OpenID Connect introduces several authentication-specific components:

- Identity Provider (IdP)
- Client Application
- User (End User)
- ID Token
- Access Token
- UserInfo Endpoint
- Discovery Document
- JWKS Endpoint

Each component participates in establishing trusted user identity.

---

# Root Cause

OIDC attacks occur because applications improperly validate identity tokens, trust user-controlled claims, or misconfigure authentication flows.

Common causes include:

- Missing ID Token validation
- Weak signature verification
- Improper issuer validation
- Missing audience validation
- Weak nonce validation
- Insecure redirect URI validation
- Token replay
- Misconfigured trust relationships

---

# Attack Surface

OIDC attacks commonly target:

- Single Sign-On (SSO)
- Enterprise Identity Providers
- Cloud applications
- Mobile applications
- REST APIs
- Web portals
- SaaS platforms
- Federated authentication systems

Applications relying on external identity providers inherit the security of their OIDC implementation.

---

# Common OIDC Attacks

## ID Token Forgery

Improper signature validation allows forged identity tokens.

---

## Token Replay

Previously issued ID Tokens are reused after successful authentication.

---

## Nonce Validation Bypass

Missing or incorrect nonce verification enables replay attacks.

---

## Issuer Validation Bypass

Applications accept ID Tokens from untrusted Identity Providers.

---

## Audience Validation Bypass

Applications accept tokens issued for different clients.

---

## Discovery Endpoint Abuse

Manipulation of OIDC discovery metadata causes applications to trust attacker-controlled configuration.

---

## JWKS Manipulation

Applications retrieve signing keys from attacker-controlled key endpoints.

---

## Redirect URI Manipulation

Weak redirect validation exposes authorization responses or tokens.

---

# Potential Impact

Successful OIDC attacks may allow attackers to:

- Bypass authentication
- Impersonate legitimate users
- Forge authenticated identities
- Access protected applications
- Escalate privileges
- Abuse federated authentication
- Compromise Single Sign-On environments
- Access enterprise resources

Because OIDC establishes user identity, successful attacks often result in complete account compromise.

---

# Common Indicators

Possible indicators include:

- Invalid ID Tokens accepted
- Missing nonce validation
- Missing issuer verification
- Missing audience verification
- Weak signature validation
- Unexpected Identity Providers
- Replayed authentication tokens

---

# Exploitation Requirements

Successful exploitation generally requires:

- Weak OIDC implementation
- Improper ID Token validation
- Misconfigured trust relationships
- Weak cryptographic verification

Applications correctly implementing the OIDC specification are resistant to most of these attacks.

---

# Mitigation

Recommended defenses include:

- Verify ID Token signatures
- Validate issuer (`iss`)
- Validate audience (`aud`)
- Validate expiration (`exp`)
- Validate issued-at (`iat`)
- Validate nonce values
- Validate redirect URIs
- Use HTTPS for all OIDC communication
- Validate JWKS sources
- Keep OIDC libraries updated

Applications should never trust identity claims until every validation step succeeds.

---

# Detection Methods

Security professionals identify OIDC vulnerabilities through:

- Authentication flow analysis
- ID Token manipulation
- Signature verification testing
- Discovery endpoint assessment
- JWKS validation
- Source code review
- Dynamic authentication testing

Testing evaluates whether applications correctly validate identity information received from the Identity Provider.

---

# OIDC vs OAuth 2.0

| OpenID Connect | OAuth 2.0 |
|----------------|-----------|
| Authentication protocol | Authorization framework |
| Verifies user identity | Grants delegated access |
| Uses ID Tokens | Uses Access Tokens |
| Answers "Who is the user?" | Answers "What may this client access?" |

OIDC extends OAuth by adding standardized authentication capabilities.

---

# Relationship to Other Vulnerabilities

OIDC attacks frequently initiate broader authentication compromise.

```
OIDC Misconfiguration

↓

Weak ID Token Validation

↓

Authentication Bypass

↓

Privilege Escalation

↓

Application Access

↓

Enterprise Compromise
```

OIDC vulnerabilities are often combined with OAuth weaknesses, JWT attacks, redirect manipulation, or session attacks.

---

# Real-World Examples

OIDC implementation flaws have affected:

- Enterprise Single Sign-On platforms
- Cloud identity providers
- SaaS applications
- Mobile authentication systems
- API gateways
- Federated identity services
- Corporate authentication portals

Most incidents result from implementation errors rather than flaws in the OIDC protocol.

---

# Importance in Offensive Security

Understanding OIDC attacks enables penetration testers to:

- Assess federated authentication
- Evaluate ID Token validation
- Test Identity Provider trust relationships
- Analyze discovery mechanisms
- Assess JWKS handling
- Recommend secure OpenID Connect implementations

---

> **Key Insight:** OpenID Connect establishes digital identity across applications. If an application improperly validates identity tokens or trusts unverified identity providers, attackers can authenticate as arbitrary users without ever knowing their credentials, making rigorous ID Token validation the cornerstone of secure OIDC implementations.