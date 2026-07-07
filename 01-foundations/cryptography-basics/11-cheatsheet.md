# Cryptography Cheatsheet (Security Perspective)

## Core Concepts

| Concept | Purpose | Reversible | Key Required |
|----------|----------|------------|--------------|
| Encoding | Data representation | Yes | No |
| Encryption | Confidentiality | Yes | Yes |
| Hashing | Integrity / Password Storage | No | No |
| Digital Signature | Authentication & Integrity | Verification Only | Yes (Private/Public Keys) |

---

## Symmetric Encryption

| Property | Details |
|----------|---------|
| Key Type | Shared Secret Key |
| Examples | AES, ChaCha20 |
| Advantages | Fast, efficient, suitable for large data |
| Disadvantages | Secure key distribution required |
| Common Uses | Disk encryption, VPNs, File encryption |

---

## Asymmetric Encryption

| Property | Details |
|----------|---------|
| Key Type | Public / Private Key Pair |
| Examples | RSA, ECC |
| Advantages | Secure key exchange, Digital signatures |
| Disadvantages | Slower than symmetric encryption |
| Common Uses | TLS, SSH, Certificates, Email Security |

---

## Hashing

| Algorithm | Status      | Typical Use                    |
| --------- | ----------- | ------------------------------ |
| MD5       | Broken      | Legacy systems                 |
| SHA-1     | Deprecated  | Legacy systems                 |
| SHA-256   | Secure      | Integrity verification         |
| SHA-512   | Secure      | High-security integrity checks |
| bcrypt    | Recommended | Password hashing               |
| scrypt    | Recommended | Password hashing               |
| Argon2id  | Recommended | Modern password hashing        |

---

## Digital Signatures

### Purpose

- Authentication
- Integrity
- Non-repudiation

### Common Uses

- Code Signing
- TLS Certificates
- Software Updates
- Email Security
- Document Signing

---

## Certificates & PKI

### Components

- Certificate Authority (CA)
- Registration Authority (RA)
- Digital Certificates
- Trust Chain

### Common Issues

- Expired certificates
- Self-signed certificates
- Weak validation
- Rogue CAs

---

## TLS / HTTPS

### Security Goals

- Confidentiality
- Integrity
- Authentication

### Recommended Versions

- TLS 1.2
- TLS 1.3

### Deprecated Versions

- SSL 2.0
- SSL 3.0
- TLS 1.0
- TLS 1.1

---

## Common Cryptographic Attacks

| Attack | Primary Target |
|---------|----------------|
| Brute Force | Passwords / Keys |
| Dictionary Attack | Weak Passwords |
| Rainbow Tables | Unsalted Hashes |
| Collision Attack | MD5 / SHA-1 |
| MITM | TLS / Certificates |
| TLS Downgrade | Legacy Protocols |
| Key Leakage | Secret Keys |
| Certificate Abuse | Trust Relationships |

---

## Common Misconfigurations

- Weak password hashing
- Missing password salts
- Hardcoded secrets
- Weak TLS configuration
- Deprecated protocols
- Weak cipher suites
- Disabled certificate validation
- Poor key management
- Weak random number generation
- Reused encryption keys
- Insecure encryption modes (ECB)

---

## Pentester Checklist

During a cryptographic assessment, verify:

- Password hashing algorithm
- Salt implementation
- Secret management
- Key storage
- TLS version
- Cipher suites
- Certificate validation
- Certificate trust chain
- JWT implementation
- Token security
- Encryption modes
- Random number generation

---

## Key Takeaways

- Cryptography is rarely broken mathematically.
- Most vulnerabilities arise from implementation flaws.
- Secure key management is as important as strong algorithms.
- Correct configuration determines the effectiveness of cryptographic protections.
- Modern penetration testing focuses on identifying cryptographic weaknesses in real-world deployments rather than attacking the underlying mathematics.
