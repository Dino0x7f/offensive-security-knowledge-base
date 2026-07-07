# Cryptography Basics (Security Perspective)

## Introduction

Cryptography is the science of protecting information through mathematical techniques that ensure secure communication, data integrity, authentication, and confidentiality.

From an offensive security perspective, cryptography is one of the most fundamental technologies encountered during penetration testing. Modern authentication protocols, secure communications, password storage, VPNs, APIs, cloud services, and operating systems all rely heavily on cryptographic mechanisms.

For penetration testers, the objective is rarely to "break the mathematics" behind cryptography. Instead, assessments focus on identifying weak implementations, insecure configurations, poor key management, and outdated algorithms that can undermine otherwise secure systems.

---

# Why Cryptography Matters

Nearly every modern enterprise technology depends on cryptography.

Examples include:

- HTTPS and TLS
- VPN technologies
- Password storage
- Active Directory authentication
- Kerberos
- NTLM
- SSH
- JWT tokens
- API authentication
- Digital certificates
- Cloud identity services

A solid understanding of cryptography enables penetration testers to recognize security weaknesses that may not be immediately visible.

---

# Core Security Objectives

Cryptography is designed to achieve several fundamental security goals.

## Confidentiality

Ensures that sensitive information can only be accessed by authorized parties.

Common technologies include:

- AES
- ChaCha20
- TLS
- VPN encryption

---

## Integrity

Ensures that information has not been modified during storage or transmission.

Common mechanisms include:

- Cryptographic Hash Functions
- HMAC
- Digital Signatures

---

## Authentication

Verifies the identity of users, devices, or services before granting access.

Examples include:

- Kerberos
- Certificates
- Public Key Infrastructure (PKI)
- Digital Signatures

---

## Non-Repudiation

Provides proof that a specific party performed an action and cannot later deny it.

Commonly achieved using:

- Digital Signatures
- Public Key Cryptography
- PKI

---

# Types of Cryptography

## Symmetric Encryption

Symmetric encryption uses a single shared secret key for both encryption and decryption.

### Characteristics

- Very fast
- Efficient for large amounts of data
- Requires secure key distribution

### Common Algorithms

- AES
- ChaCha20

### Typical Use Cases

- Disk encryption
- VPN tunnels
- TLS session encryption
- File encryption

---

## Asymmetric Encryption

Asymmetric encryption uses a pair of mathematically related keys:

- Public Key
- Private Key

### Characteristics

- More computationally expensive
- Solves the key distribution problem
- Enables digital signatures

### Common Algorithms

- RSA
- ECC

### Typical Use Cases

- TLS handshake
- SSH authentication
- Digital certificates
- PKI
- Secure key exchange

---

## Cryptographic Hash Functions

Hash functions transform data into a fixed-length value.

Unlike encryption, hashing is a one-way operation.

### Characteristics

- Cannot be reversed
- Deterministic
- Fixed output length
- Sensitive to input changes

### Common Algorithms

- SHA-256
- SHA-384
- SHA-512

### Weak Algorithms

- MD5
- SHA-1

### Typical Use Cases

- Password storage
- File integrity verification
- Digital signatures
- Certificate validation

---

# Common Cryptographic Components

During penetration testing, cryptography appears in many different forms.

Examples include:

- Password Hashes
- TLS Certificates
- Kerberos Tickets
- JWT Tokens
- API Keys
- SSH Keys
- VPN Credentials
- Digital Certificates

Understanding how these components work is essential for identifying implementation weaknesses.

---

# Common Security Failures

Most real-world cryptographic vulnerabilities result from implementation mistakes rather than weaknesses in the algorithms themselves.

Common examples include:

- Weak password hashing algorithms
- Unsalted password hashes
- Hardcoded encryption keys
- Reused encryption keys
- Weak random number generation
- Insecure TLS configurations
- Expired or invalid certificates
- Poor certificate validation
- Insecure key storage

---

# Pentester Perspective

During security assessments, penetration testers evaluate cryptographic implementations by asking questions such as:

- Which algorithms are being used?
- Are deprecated algorithms still enabled?
- How are encryption keys generated and stored?
- Are password hashes resistant to offline attacks?
- Is TLS configured securely?
- Are certificates properly validated?
- Can authentication tokens be forged or reused?

The objective is to identify weaknesses that reduce the effectiveness of cryptographic protections.

---

# Common Misconceptions

Several misconceptions frequently arise:

- Encryption makes systems completely secure.
- Strong algorithms cannot be compromised.
- HTTPS automatically guarantees security.
- Hashing and encryption are interchangeable.

In practice, secure cryptography depends on proper implementation, configuration, and key management.

---

# Key Takeaways

- Cryptography protects confidentiality, integrity, authentication, and non-repudiation.
- Modern enterprise environments rely heavily on cryptographic technologies.
- Most cryptographic vulnerabilities result from implementation errors rather than broken algorithms.
- Penetration testers assess how cryptography is implemented, configured, and managed.
- Understanding cryptographic fundamentals is essential before studying authentication protocols, Active Directory, web security, and enterprise attack chains.
