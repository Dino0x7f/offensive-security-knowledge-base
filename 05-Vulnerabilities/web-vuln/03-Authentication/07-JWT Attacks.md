# JWT Attacks

## Overview

JWT (JSON Web Token) Attacks are a class of authentication vulnerabilities that target insecure implementations of JSON Web Tokens used for stateless authentication and authorization.

A JSON Web Token contains claims about a user and is digitally signed to ensure integrity. If the application improperly validates, signs, or processes JWTs, attackers may forge tokens, bypass authentication, escalate privileges, or impersonate other users.

JWT attacks rarely target the JWT standard itself. Instead, they exploit incorrect implementations, insecure configurations, or flawed validation logic.

---

# JWT Authentication Flow

Typical authentication process:

```
User Authenticates

↓

Server Generates JWT

↓

JWT Returned to Client

↓

Client Sends JWT

↓

Server Verifies Signature

↓

Access Granted
```

Security depends on correctly validating every received token.

---

# JWT Structure

A JWT consists of three parts:

```
Header

↓

Payload

↓

Signature
```

- **Header** — Defines token type and signing algorithm.
- **Payload** — Contains claims about the user.
- **Signature** — Protects token integrity and authenticity.

Only the signature provides security; the header and payload are Base64URL-encoded, not encrypted.

---

# Root Cause

JWT attacks occur because applications incorrectly validate or trust JWTs.

Common causes include:

- Missing signature verification
- Weak signing secrets
- Algorithm confusion
- Insecure key management
- Excessive trust in client-controlled claims
- Improper token expiration validation
- Missing audience or issuer validation
- Weak JWT libraries

---

# Attack Surface

JWT attacks commonly target:

- REST APIs
- GraphQL APIs
- Mobile applications
- Single Page Applications (SPAs)
- OAuth implementations
- OpenID Connect
- Microservices
- Cloud applications

JWTs are widely used wherever stateless authentication is required.

---

# Common JWT Attacks

## Weak Secret Keys

Attackers brute-force weak HMAC signing secrets and forge valid tokens.

---

## Algorithm Confusion

Applications incorrectly trust attacker-controlled algorithm values.

---

## Missing Signature Validation

The server accepts tokens without verifying their signatures.

---

## None Algorithm Attack

The application accepts unsigned JWTs using the `"none"` algorithm.

---

## Claim Manipulation

Attackers modify user-controlled claims that are not properly validated.

---

## Expired Token Acceptance

Applications ignore expiration timestamps.

---

## Key Confusion

Incorrect handling of public and private keys allows signature forgery.

---

## Information Disclosure

Sensitive information is stored inside JWT payloads without encryption.

---

# Potential Impact

Successful JWT attacks may allow attackers to:

- Bypass authentication
- Impersonate users
- Escalate privileges
- Access protected APIs
- Forge administrator tokens
- Modify authorization claims
- Maintain persistent authentication
- Compromise distributed systems

The impact depends on how much trust the application places in JWT contents.

---

# Common Indicators

Possible indicators include:

- JWT accepted after payload modification
- Weak signing secrets
- Unsigned JWT acceptance
- Missing expiration enforcement
- Missing issuer validation
- Missing audience validation
- Excessive client-controlled claims

---

# Exploitation Requirements

Successful exploitation generally requires:

- Improper JWT validation
- Weak cryptographic implementation
- Application trust in modified claims
- Incorrect key management

Correctly implemented JWT authentication is resistant to these attacks.

---

# Mitigation

Recommended defenses include:

- Always verify JWT signatures
- Use strong cryptographic keys
- Reject the `"none"` algorithm
- Enforce expiration validation
- Validate issuer (`iss`)
- Validate audience (`aud`)
- Validate issued-at (`iat`) and not-before (`nbf`) claims
- Store minimal information inside tokens
- Rotate signing keys regularly
- Keep JWT libraries updated

Applications should treat every received JWT as untrusted until fully validated.

---

# Detection Methods

Security professionals identify JWT vulnerabilities through:

- Manual token manipulation
- Signature verification testing
- Secret strength assessment
- Source code review
- API security testing
- Automated vulnerability scanners

Testing focuses on whether modified or forged tokens are accepted by the application.

---

# JWT vs Session Cookies

| JWT | Session Cookie |
|------|----------------|
| Stateless authentication | Stateful authentication |
| Stores claims inside the token | Stores session identifier only |
| Server verifies signature | Server verifies session state |
| No server-side session required | Server maintains session data |

Both mechanisms provide authentication but require different security considerations.

---

# Relationship to Other Vulnerabilities

JWT attacks often lead directly to authentication bypass.

```
Weak JWT Validation

↓

Forged Token

↓

Authentication Bypass

↓

Privilege Escalation

↓

Protected Resource Access

↓

Application Compromise
```

JWT vulnerabilities are frequently combined with Broken Access Control or insecure API authorization.

---

# Real-World Examples

JWT vulnerabilities have affected:

- REST APIs
- Mobile backends
- Cloud platforms
- Single Page Applications
- OAuth services
- Identity providers
- Enterprise APIs
- Microservice architectures

Many historical incidents resulted from applications accepting forged or improperly validated JWTs rather than flaws in the JWT specification itself.

---

# Importance in Offensive Security

Understanding JWT attacks enables penetration testers to:

- Assess stateless authentication
- Evaluate token validation logic
- Test signature verification
- Analyze claim handling
- Assess cryptographic key management
- Recommend secure JWT implementation practices

---

> **Key Insight:** JWTs are secure only when every aspect of their validation is correctly implemented. The signature not the payload establishes trust. If an application accepts modified, forged, expired, or improperly validated tokens, attackers can often bypass authentication entirely without ever knowing a user's credentials.