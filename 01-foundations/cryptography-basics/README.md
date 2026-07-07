# Cryptography Basics

## Overview

This section introduces the fundamental concepts of cryptography from an offensive security perspective.

The goal is to understand how modern systems protect data, how trust is established, and how attackers exploit weaknesses in cryptographic implementations rather than breaking the underlying mathematics.

---

## Contents

### Cryptography Basics
Introduction to cryptography, its goals, and its role in modern security.

### Cryptography Fundamentals
Core concepts including plaintext, ciphertext, keys, and cryptographic algorithms.

### Symmetric Encryption
Understanding shared-key encryption, common algorithms, and implementation risks.

### Asymmetric Encryption
Public-key cryptography, key exchange, digital signatures, and common attack surfaces.

### Hashing
One-way cryptographic functions, password storage, and integrity verification.

### Encoding vs Encryption
Understanding the differences between encoding, encryption, and hashing.

### Digital Signatures
Authenticity, integrity, and non-repudiation using asymmetric cryptography.

### Certificates and PKI
Certificate authorities, trust chains, and Public Key Infrastructure.

### TLS / SSL Basics
Secure communication, TLS handshake, certificates, and common misconfigurations.

### Common Cryptography Attacks
Brute force, dictionary attacks, rainbow tables, MITM, downgrade attacks, and key leakage.

### Cryptography Misconfigurations
Weak algorithms, poor key management, insecure TLS settings, and implementation mistakes.

### Pentester Perspective
How penetration testers assess cryptographic implementations and identify practical weaknesses.

### Cryptography Cheatsheet
A quick reference covering the essential concepts and common security issues.

---

## Learning Objectives

After completing this section, you should be able to:

- Understand the goals of cryptography
- Distinguish between encryption, hashing, and encoding
- Explain symmetric and asymmetric encryption
- Understand digital signatures and PKI
- Identify common cryptographic attacks
- Recognize implementation flaws and misconfigurations
- Evaluate cryptographic security from a pentester's perspective

---

## Security Mindset

Strong cryptographic algorithms rarely fail.

Most real-world compromises occur because of:
- Weak implementations
- Poor key management
- Misconfigurations
- Human mistakes

---

## Key Takeaway

> Cryptography is not about hiding data—it is about establishing confidentiality, integrity, authenticity, and trust. In offensive security, attackers target how cryptography is implemented, not the mathematics behind it.
