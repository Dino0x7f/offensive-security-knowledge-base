# Hash Functions (Security Perspective)

## Introduction

A cryptographic hash function is a one-way mathematical algorithm that transforms data of any size into a fixed-length value known as a hash or digest.

Unlike encryption, hashing is irreversible. The original input cannot be recovered from the resulting hash.

From a penetration testing perspective, hash functions are fundamental to authentication, integrity verification, digital signatures, and password storage. Understanding how hash functions work is essential for identifying weak cryptographic implementations.

---

# How Hashing Works

A hash function accepts arbitrary input and produces a fixed-length output.

```text
Input Data
      │
      ▼
Hash Function
      │
      ▼
Hash (Digest)
```

Changing even a single bit of the input produces a completely different hash value.

---

# Properties of Cryptographic Hash Functions

A secure hash function should provide the following properties:

## Deterministic

The same input always produces the same output.

---

## One-Way

Recovering the original input from the hash should be computationally infeasible.

---

## Fixed Output Length

Regardless of input size, the output length remains constant.

---

## Avalanche Effect

A tiny change in the input should generate a significantly different hash.

---

## Collision Resistance

It should be extremely difficult to find two different inputs that produce the same hash.

---

# Common Hash Algorithms

## MD5

- 128-bit output
- Fast
- Cryptographically broken
- Vulnerable to collision attacks

Should never be used for security-sensitive applications.

---

## SHA-1

- 160-bit output
- Deprecated
- Collision attacks are practical

No longer recommended for modern systems.

---

## SHA-256

Part of the SHA-2 family.

Widely used for:

- File integrity
- Certificates
- Digital signatures
- Blockchain
- Secure communications

---

## SHA-512

Another SHA-2 algorithm offering a larger digest and increased security margin.

Commonly used in enterprise environments.

---

# Common Use Cases

Hash functions are used throughout modern computing.

Examples include:

- File integrity verification
- Digital signatures
- Certificate validation
- Software verification
- Blockchain technologies
- Authentication protocols
- Password storage (through dedicated password hashing algorithms)

---

# Common Security Risks

## Weak Algorithms

Using deprecated algorithms such as MD5 or SHA-1 introduces significant security risks.

---

## Collision Attacks

Attackers generate two different inputs that produce the same hash value.

This undermines integrity verification and digital signatures.

---

## Incorrect Password Storage

General-purpose hash functions such as SHA-256 should not be used directly for password storage.

Dedicated password hashing algorithms provide significantly stronger protection.

---

## Integrity Validation Failures

Applications that fail to verify hashes correctly may accept modified files or malicious updates.

---

# Pentester Perspective

During assessments, penetration testers evaluate:

- Which hash algorithms are in use.
- Whether deprecated algorithms remain enabled.
- Whether integrity verification is correctly implemented.
- Whether password storage relies on appropriate hashing mechanisms.
- Whether digital signatures depend on secure hash functions.

The objective is not to reverse hashes but to identify weak cryptographic choices and insecure implementations.

---

# Common Misconceptions

Several misconceptions frequently arise:

- Hashing is encryption.
- Hashes can always be reversed.
- SHA-256 is suitable for password storage.
- Strong hash algorithms alone guarantee secure authentication.

In reality, secure password storage requires specialized password hashing algorithms rather than general-purpose hash functions.

---

# Key Takeaways

- Hash functions are one-way mathematical operations.
- They provide integrity verification rather than confidentiality.
- MD5 and SHA-1 are no longer considered secure.
- SHA-256 and SHA-512 remain widely used for integrity and digital signatures.
- Password storage should rely on dedicated password hashing algorithms, which are covered separately.
