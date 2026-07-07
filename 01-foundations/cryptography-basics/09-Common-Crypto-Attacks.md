# Common Cryptographic Attacks (Security Perspective)

## Introduction

Modern cryptographic algorithms such as AES, RSA, and SHA-2 are considered computationally secure when implemented correctly.

In practice, attackers rarely attempt to break these algorithms directly. Instead, they exploit weaknesses in implementation, key management, protocol design, or human error.

From a penetration testing perspective, understanding cryptographic attacks is primarily about identifying insecure deployments rather than defeating cryptographic mathematics.

---

# Categories of Cryptographic Attacks

Cryptographic attacks generally target one or more of the following:

- Weak passwords
- Poor key management
- Insecure implementations
- Misconfigured protocols
- Weak randomness
- Trust validation failures

---

# Brute Force Attacks

## Overview

A brute force attack systematically attempts every possible key or password until the correct one is found.

---

## Common Targets

- User passwords
- Encrypted archives
- SSH credentials
- VPN authentication
- Weak encryption keys

---

## Defensive Controls

- Strong passwords
- Long cryptographic keys
- Multi-Factor Authentication (MFA)
- Rate limiting
- Account lockout policies

---

# Dictionary Attacks

## Overview

Dictionary attacks attempt authentication using predefined wordlists rather than every possible combination.

---

## Common Targets

- Password hashes
- Login portals
- ZIP archives
- SSH services

---

## Defensive Controls

- Strong password policies
- Password managers
- Password complexity
- MFA

---

# Rainbow Table Attacks

## Overview

Rainbow tables are precomputed databases of password hashes used to accelerate password recovery.

---

## Requirements

Rainbow table attacks are effective only when:

- Passwords are unsalted
- Weak hash algorithms are used
- Fast hash functions are used

---

## Defensive Controls

- Unique salts
- Argon2
- bcrypt
- scrypt
- PBKDF2

---

# Collision Attacks

## Overview

Collision attacks generate two different inputs that produce the same hash value.

---

## Affected Algorithms

- MD5
- SHA-1

---

## Real-World Impact

Collision attacks may compromise:

- Digital signatures
- File integrity verification
- Certificate validation

---

# Man-in-the-Middle (MITM)

## Overview

An attacker intercepts communication between two trusted parties.

---

## Common Attack Vectors

- ARP Spoofing
- Rogue Wi-Fi
- DNS Spoofing
- Weak TLS validation

---

## Defensive Controls

- TLS
- Certificate validation
- HSTS
- VPNs

---

# TLS Downgrade Attacks

## Overview

The attacker forces both parties to negotiate weaker cryptographic protocols.

---

## Historical Examples

- POODLE
- BEAST
- FREAK
- Logjam

---

## Defensive Controls

- TLS 1.2+
- TLS 1.3
- Disable legacy protocols

---

# Key Leakage

## Overview

Cryptographic systems fail immediately when secret keys become exposed.

---

## Common Sources

- Git repositories
- Source code
- Configuration files
- Memory dumps
- Backup files
- Environment variables

---

## Defensive Controls

- Hardware Security Modules (HSM)
- Secret management systems
- Key rotation
- Access control

---

# Weak Random Number Generation

## Overview

Many cryptographic systems depend on secure random values.

Weak randomness may produce predictable:

- Session IDs
- Password reset tokens
- Encryption keys
- JWT secrets
- Nonces

---

## Defensive Controls

- Cryptographically Secure Random Number Generators (CSPRNG)

---

# Certificate Attacks

## Overview

Rather than attacking encryption itself, attackers frequently abuse certificate trust.

---

## Examples

- Rogue certificates
- Self-signed certificates
- Expired certificates
- Weak certificate validation
- Compromised Certificate Authorities

---

## Defensive Controls

- Proper certificate validation
- Certificate pinning (where appropriate)
- Certificate revocation checking

---

# Cryptographic Implementation Failures

Most real-world vulnerabilities originate from implementation mistakes.

Common examples include:

- Hardcoded encryption keys
- Reused initialization vectors (IVs)
- ECB encryption mode
- Missing authentication
- Improper key storage
- Broken JWT verification
- Disabled certificate validation
- Insecure API implementations

---

# Pentester Perspective

During assessments, penetration testers evaluate:

- Supported cryptographic algorithms
- Password hashing mechanisms
- TLS configuration
- Certificate trust chains
- Secret management
- Key storage
- Random number generation
- Authentication protocols
- Cryptographic libraries
- Enterprise PKI deployments

The objective is to identify weaknesses that allow attackers to bypass, misuse, or undermine cryptographic protections.

---

# Common Misconceptions

Several misconceptions frequently arise:

- AES can easily be broken.
- Base64 is encryption.
- HTTPS guarantees a secure application.
- Hashes can always be decrypted.
- Strong algorithms alone guarantee security.

In reality, secure cryptography depends primarily on correct implementation, secure key management, and proper protocol configuration.

---

# Key Takeaways

- Modern cryptographic algorithms are rarely the weakest component.
- Most successful attacks target implementation flaws rather than mathematical weaknesses.
- Weak passwords, exposed keys, insecure TLS configurations, and poor certificate management remain common attack vectors.
- Understanding cryptographic attacks enables penetration testers to identify high-impact vulnerabilities before attackers exploit them.
