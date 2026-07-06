# HTTP & HTTPS (Security Perspective)

## Introduction

HTTP (Hypertext Transfer Protocol) is the application-layer protocol used for communication between clients (such as web browsers) and web servers.

From a security perspective:

> HTTP defines how web applications exchange data, making it one of the most important protocols for both attackers and defenders.

Because nearly every web application relies on HTTP, understanding its behavior is fundamental to web application security testing.

---

# HTTP

HTTP transfers requests and responses in plaintext.

Unless additional protection is applied, anyone with access to the communication channel can potentially observe or modify transmitted data.

### Characteristics

- Stateless protocol
- Request-response communication model
- Human-readable messages
- No built-in encryption
- No built-in integrity protection
- No built-in authentication

---

# HTTPS

HTTPS is HTTP encapsulated within TLS (Transport Layer Security).

TLS establishes a secure communication channel before HTTP messages are exchanged.

HTTPS provides:

- Confidentiality (encryption)
- Integrity (tamper detection)
- Server authentication through digital certificates

---

# HTTP vs HTTPS

| Feature | HTTP | HTTPS |
|---------|-------|--------|
| Encryption | No | Yes |
| Integrity Protection | No | Yes |
| Server Authentication | No | Yes |
| Default Port | 80 | 443 |
| Traffic Visibility | Plaintext | Encrypted |

---

# Why HTTP Matters in Web Security

Every web assessment revolves around HTTP communication.

Pentesters analyze:

- HTTP requests
- HTTP responses
- Headers
- Cookies
- Parameters
- Status codes
- Methods
- Session identifiers

Nearly every web vulnerability is ultimately exposed through manipulated HTTP requests.

---

# Common Security Risks

## HTTP

Without encryption, attackers may perform:

- Credential interception
- Session cookie theft
- Man-in-the-Middle (MITM) attacks
- Traffic manipulation
- Information disclosure

---

## HTTPS

Although HTTPS secures transport, weaknesses still occur through:

- Weak TLS configurations
- Deprecated protocol versions
- Weak cipher suites
- Invalid or expired certificates
- Improper certificate validation
- TLS downgrade attacks

These weaknesses affect transport security rather than application security.

---

# Pentester Perspective

During an assessment, HTTP traffic is examined to identify:

- Authentication mechanisms
- Session handling
- Authorization controls
- Input parameters
- Hidden functionality
- API endpoints
- Security headers
- Sensitive information leakage

The HTTP request is often the primary object of manipulation during web penetration testing.

---

# Common Misconceptions

HTTPS **does not**:

- Prevent SQL Injection
- Prevent Cross-Site Scripting (XSS)
- Prevent Broken Access Control
- Prevent IDOR
- Prevent Server-Side Request Forgery (SSRF)

It only protects the communication channel between client and server.

---

# Key Takeaways

- HTTP is the foundation of nearly all web communication.
- HTTPS secures data in transit using TLS.
- Most web attacks manipulate HTTP requests rather than the protocol itself.
- Secure transport does not imply a secure application.

---

# Key Insight

> HTTPS protects how data travels across the network it does not protect how the application processes that data. Secure transport and secure application logic are two separate security concerns.
