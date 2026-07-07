# Cryptography Fundamentals (Security Perspective)

## Introduction

Cryptography relies on mathematical principles that protect information from unauthorized access, modification, or forgery.

Before studying encryption algorithms or authentication protocols, it is essential to understand the fundamental concepts that form the basis of modern cryptographic systems.

From a penetration testing perspective, these concepts help explain how secure communication is established, how credentials are protected, and where implementation mistakes may introduce vulnerabilities.

---

# Core Cryptographic Components

## Plaintext

Plaintext is the original readable information before any cryptographic operation is applied.

Examples include:

- Passwords
- Documents
- API requests
- Email messages

---

## Ciphertext

Ciphertext is the encrypted form of plaintext.

Without the appropriate decryption key, ciphertext should be computationally infeasible to recover.

The objective of encryption is to transform readable information into ciphertext while preserving confidentiality.

---

## Cryptographic Key

A cryptographic key is a secret value used by an algorithm to encrypt, decrypt, sign, or verify information.

The security of most cryptographic systems depends primarily on protecting the secrecy of cryptographic keys rather than hiding the algorithms themselves.

Examples include:

- AES keys
- RSA private keys
- SSH keys
- TLS session keys

---

## Cryptographic Algorithm

A cryptographic algorithm defines the mathematical operations performed on data.

Algorithms are generally public and widely studied.

Security relies on the strength of the algorithm together with proper key management—not on keeping the algorithm secret.

Examples include:

- AES
- RSA
- ECC
- SHA-256

---

# Fundamental Security Properties

Modern cryptography aims to provide several essential security properties.

## Confidentiality

Ensures that information can only be accessed by authorized parties.

---

## Integrity

Ensures that information has not been modified without authorization.

---

## Authentication

Verifies the identity of users, systems, or services.

---

## Non-Repudiation

Provides proof that an action was performed by a specific entity and cannot later be denied.

---

# Where Cryptography Appears

Cryptography is integrated into almost every modern technology.

Common examples include:

- HTTPS / TLS
- VPN connections
- Password storage
- SSH
- Kerberos
- JWT Tokens
- Digital Certificates
- Public Key Infrastructure (PKI)
- Cloud Identity Services
- Secure APIs

---

# Common Implementation Mistakes

Most cryptographic weaknesses originate from poor implementation rather than flawed mathematics.

Common examples include:

- Weak hashing algorithms
- Hardcoded secrets
- Predictable random values
- Reused encryption keys
- Missing password salts
- Weak TLS configurations
- Poor certificate validation
- Insecure key storage

---

# Pentester Perspective

During security assessments, penetration testers evaluate:

- Which cryptographic algorithms are in use.
- Whether deprecated algorithms remain enabled.
- How encryption keys are generated and protected.
- Whether certificates are properly validated.
- Whether authentication tokens can be abused.
- Whether password storage follows modern security practices.

Understanding these fundamentals allows penetration testers to identify weaknesses that may compromise otherwise secure systems.

---

# Common Misconceptions

Several misconceptions frequently arise:

- Strong encryption guarantees complete security.
- Secret algorithms are more secure.
- Hashing and encryption serve the same purpose.
- HTTPS eliminates every security risk.

In reality, cryptographic security depends on proper implementation, configuration, and key management.

---

# Key Takeaways

- Cryptography protects information through mathematical techniques and secure key management.
- Plaintext, ciphertext, keys, and algorithms are the foundation of every cryptographic system.
- Modern enterprise technologies rely heavily on cryptography for authentication and secure communication.
- Most real-world vulnerabilities arise from implementation errors rather than broken algorithms.
- Understanding cryptographic fundamentals is essential before studying encryption algorithms, hashing, TLS, Kerberos, and Active Directory authentication.
