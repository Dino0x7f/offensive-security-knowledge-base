# Cryptographic Misconfigurations (Security Perspective)

## Introduction

Modern cryptographic algorithms are highly secure when implemented correctly.

However, real-world compromises rarely result from broken cryptography. Instead, they stem from weak configurations, poor key management, insecure implementations, and incorrect assumptions.

From a penetration testing perspective, cryptographic misconfigurations often provide attackers with opportunities to bypass authentication, decrypt sensitive information, or impersonate trusted systems.

---

# Weak Password Hashing

## Overview

Using fast cryptographic hash functions for password storage significantly weakens authentication security.

Common examples include:

- MD5
- SHA-1
- SHA-256 (without a password hashing algorithm)

---

## Security Impact

- Fast offline password cracking
- Dictionary attacks
- Rainbow table attacks
- Credential stuffing

---

## Recommended Practice

Use dedicated password hashing algorithms such as:

- Argon2id
- bcrypt
- scrypt
- PBKDF2

---

# Missing Salt

## Overview

A salt is a unique random value added to every password before hashing.

Without salts:

- Identical passwords generate identical hashes.
- Precomputed attacks become practical.

---

## Security Impact

- Rainbow table attacks
- Password reuse identification
- Large-scale credential cracking

---

# Hardcoded Secrets

## Overview

Embedding cryptographic keys inside applications is a common development mistake.

Examples include:

- Source code
- Mobile applications
- Configuration files
- Docker images
- Git repositories

---

## Security Impact

- Complete compromise of encrypted data
- Token forgery
- Authentication bypass

---

# Weak TLS Configuration

Common examples include:

- SSL enabled
- TLS 1.0 or TLS 1.1 enabled
- Weak cipher suites
- RC4 support
- 3DES support

---

## Security Impact

- Downgrade attacks
- Weak encryption
- Man-in-the-Middle attacks

---

# Improper Certificate Validation

Applications sometimes:

- Accept any certificate
- Ignore hostname validation
- Trust self-signed certificates
- Disable certificate verification

---

## Security Impact

- Man-in-the-Middle attacks
- Server impersonation
- Credential interception

---

# Weak Random Number Generation

Many cryptographic systems require unpredictable random values.

Weak randomness affects:

- Session IDs
- JWT secrets
- API tokens
- Password reset links
- Encryption keys

---

## Security Impact

- Predictable authentication tokens
- Session hijacking
- Key prediction

---

# Poor Key Management

Common issues include:

- Plaintext private keys
- Shared keys across systems
- No key rotation
- Weak access controls
- Unencrypted backups

---

## Security Impact

- Credential compromise
- Decryption of sensitive data
- Certificate abuse

---

# Insecure Encryption Modes

Incorrect encryption modes significantly reduce security.

Common examples:

- AES-ECB
- Reused Initialization Vectors (IVs)
- Static IVs

---

## Security Impact

- Pattern leakage
- Predictable ciphertext
- Information disclosure

---

# Weak Cryptographic Libraries

Organizations sometimes continue using:

- Deprecated libraries
- Unpatched cryptographic implementations
- Vulnerable OpenSSL versions

---

## Security Impact

- Known public vulnerabilities
- Remote compromise
- TLS failures

---

# Pentester Perspective

During assessments, penetration testers evaluate:

- Password hashing algorithms
- Salt implementation
- Secret storage
- TLS configuration
- Certificate validation
- Cipher suites
- Key management
- Encryption modes
- Cryptographic libraries
- Random number generation

The goal is to identify weaknesses that reduce or completely eliminate the protection cryptography is intended to provide.

---

# Common Misconceptions

Several misconceptions frequently arise:

- AES automatically secures an application.
- HTTPS guarantees secure communication.
- SHA-256 is sufficient for password storage.
- Hardcoded keys are acceptable in internal systems.
- Self-signed certificates are harmless.

These assumptions frequently lead to serious security vulnerabilities.

---

# Key Takeaways

- Strong cryptographic algorithms cannot compensate for weak implementations.
- Most cryptographic vulnerabilities originate from configuration and operational mistakes.
- Secure key management is as important as the encryption algorithm itself.
- Penetration testers should evaluate the entire cryptographic ecosystem rather than focusing solely on algorithms.

