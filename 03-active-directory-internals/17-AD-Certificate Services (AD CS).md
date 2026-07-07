# Active Directory Certificate Services (AD CS)

## Overview

**Active Directory Certificate Services (AD CS)** is Microsoft's Public Key Infrastructure (PKI) solution for enterprise environments. It enables organizations to issue, manage, renew, and revoke digital certificates used for authentication, encryption, digital signatures, and secure communications.

From a security perspective:

> AD CS extends trust across the enterprise. A compromised Certificate Authority can be as impactful as a compromised Domain Controller because certificates can be used to authenticate users and systems without passwords.

Understanding AD CS is essential for:

- Active Directory Security
- PKI
- Enterprise Authentication
- Smart Card Authentication
- Kerberos PKINIT
- Red Team Operations
- Incident Response

---

# What is AD CS?

AD CS is Microsoft's implementation of a Public Key Infrastructure (PKI).

It provides:

- Certificate Authorities (CA)
- Certificate Templates
- Certificate Enrollment
- Certificate Revocation
- Certificate Lifecycle Management

---

# High-Level Architecture

```
User / Computer

↓

Certificate Request

↓

Enterprise CA

↓

Certificate Issued

↓

Authentication
```

Certificates become trusted credentials within the domain.

---

# Why AD CS Exists

Passwords have limitations.

Certificates provide:

- Strong authentication
- Passwordless authentication
- Digital signatures
- Encryption
- Identity verification

---

# Core Components

An AD CS deployment typically includes:

```
AD CS

│

├── Certificate Authority (CA)

├── Certificate Templates

├── Certificate Enrollment

├── Certificate Revocation

└── Certificate Database
```

---

# Certificate Authority (CA)

The Certificate Authority is responsible for:

- Issuing certificates
- Validating requests
- Revoking certificates
- Managing trust

Every certificate is signed by the CA.

---

# Root CA

The Root CA is the trust anchor.

```
Root CA

↓

Signs

↓

Subordinate CAs

↓

Issue Certificates
```

Compromising the Root CA compromises the entire PKI.

---

# Enterprise CA

An Enterprise CA:

- Integrates with Active Directory
- Publishes templates
- Supports auto-enrollment
- Uses domain authentication

Most enterprise environments deploy Enterprise CAs.

---

# Standalone CA

A Standalone CA:

- Does not integrate with AD
- Requires manual approval
- Uses fewer automated features

Typically used for isolated environments.

---

# Certificate Templates

Templates define:

- Who may enroll
- Certificate purpose
- Key size
- Validity period
- Authentication methods
- Enrollment permissions

Templates are central to AD CS security.

---

# Certificate Enrollment

Enrollment process:

```
User

↓

Certificate Request

↓

Certificate Authority

↓

Certificate Issued

↓

Stored in User Certificate Store
```

---

# Auto-Enrollment

Active Directory supports:

```
Automatic Certificate Enrollment
```

Example:

```
Computer Joins Domain

↓

Group Policy

↓

Certificate Requested

↓

Certificate Installed
```

No administrator interaction is required.

---

# Certificate Contents

A certificate typically contains:

- Subject
- Public Key
- Issuer
- Serial Number
- Validity Period
- Extensions
- Digital Signature

---

# Public Key Infrastructure (PKI)

AD CS implements PKI.

```
Private Key

+

Public Key

↓

Certificate

↓

Trust
```

The private key remains secret.

The public key is distributed through the certificate.

---

# Certificate Authentication

Instead of:

```
Username

+

Password
```

Authentication becomes:

```
Private Key

↓

Certificate

↓

Authentication
```

This enables passwordless authentication.

---

# PKINIT

Kerberos supports:

```
PKINIT

(Public Key Cryptography for Initial Authentication)
```

Authentication flow:

```
Certificate

↓

Kerberos

↓

TGT

↓

Authentication
```

Users authenticate with certificates rather than passwords.

---

# Smart Card Authentication

Smart Cards store:

- Private Keys
- Certificates

Authentication becomes:

```
Smart Card

↓

Certificate

↓

Kerberos

↓

Access Granted
```

Widely used in high-security environments.

---

# Certificate Revocation

Certificates may be revoked if:

- Private key compromised
- Employee leaves organization
- Device stolen
- Certificate misissued

Revoked certificates should no longer be trusted.

---

# CRL (Certificate Revocation List)

The CA publishes a:

```
Certificate Revocation List

(CRL)
```

Clients verify that certificates have not been revoked before trusting them.

---

# OCSP

Instead of downloading the entire CRL:

Clients may query:

```
Online Certificate Status Protocol

(OCSP)
```

For real-time revocation status.

---

# Security Importance

AD CS enables:

- Enterprise authentication
- VPN authentication
- Wi-Fi authentication
- TLS certificates
- Smart Cards
- Device authentication
- Secure email

Many enterprise services depend on certificates.

---

# Offensive Perspective

AD CS has become one of the most valuable attack surfaces in Active Directory.

Attackers target:

- Certificate Templates
- Enterprise CAs
- Enrollment permissions
- Certificate mapping
- PKINIT

A forged or improperly issued certificate can enable long-term persistence.

---

# Common AD CS Attacks

Examples include:

| Attack | Description |
|----------|-------------|
| ESC1 | Misconfigured enrollment permissions |
| ESC2 | Dangerous certificate templates |
| ESC3 | Enrollment Agent abuse |
| ESC4 | Template modification |
| ESC6 | SAN abuse |
| ESC8 | NTLM relay to AD CS |
| Golden Certificate | Forged certificate using compromised CA key |

These attack paths are collectively known as **ESC (Enterprise Security Configuration)** abuses.

---

# Common Attack Targets

| Target | Objective |
|----------|-----------|
| Enterprise CA | Enterprise compromise |
| Certificate Templates | Privilege escalation |
| Enrollment Permissions | Unauthorized certificates |
| CA Private Key | Golden Certificates |
| PKINIT | Passwordless authentication abuse |

---

# Common Misconfigurations

Examples include:

- Authenticated Users allowed to enroll in privileged templates
- Client Authentication enabled on inappropriate templates
- Weak template permissions
- Long certificate validity periods
- Poor CA protection
- Unrestricted Enrollment Agent certificates

Misconfigurations can allow attackers to obtain certificates for privileged identities.

---

# Defensive Perspective

Administrators should:

- Audit certificate templates
- Restrict enrollment permissions
- Protect CA private keys
- Monitor certificate issuance
- Review template ACLs
- Enable logging on CA servers
- Regularly audit AD CS configuration

Certificate Authorities should be treated as Tier-0 assets.

---

# AD CS vs Kerberos

| AD CS | Kerberos |
|--------|-----------|
| Issues certificates | Issues tickets |
| Uses PKI | Uses symmetric cryptography |
| Supports passwordless authentication | Uses passwords or certificates (PKINIT) |
| Long-term trust | Session-based authentication |

The two technologies complement each other in Active Directory.

---

# Why AD CS Matters

Understanding AD CS enables security professionals to:

- Secure enterprise PKI
- Detect certificate abuse
- Investigate authentication anomalies
- Understand passwordless authentication
- Assess AD CS attack paths
- Defend against certificate-based persistence

---

# Key Takeaways

- AD CS is Microsoft's enterprise Public Key Infrastructure.
- Certificate Authorities establish trust by issuing signed certificates.
- Certificate Templates define who can request certificates and for what purpose.
- PKINIT allows Kerberos authentication using certificates instead of passwords.
- AD CS misconfigurations can lead to privilege escalation and persistent domain compromise.
- Enterprise CAs should be protected with the same level of security as Domain Controllers.

---

# Key Insight

> Active Directory Certificate Services extends trust beyond passwords. A valid certificate can authenticate a user, computer, or service across the enterprise, making AD CS one of the most powerful and most frequently overlooked—security components in modern Windows environments. Securing the Certificate Authority and its templates is critical to protecting the entire Active Directory infrastructure.
