# Web Security Cheatsheet (Security Perspective)

## Web Security Mindset

> Never trust client input. Trust must always be verified on the server.

A web application is an input-processing system where every request may become an attack.

---

# Core Concepts

- Input is always untrusted.
- Client-side validation is bypassable.
- Server-side validation is mandatory.
- Authentication verifies identity.
- Authorization verifies permissions.
- Sessions maintain user state.
- APIs expose backend functionality.
- Every request crosses one or more trust boundaries.

---

# Web Architecture

```
Browser
    │
    ▼
Web Server
    │
    ▼
Application Logic
    │
    ▼
Database
```

### Trust Boundaries

- Client → Server
- Server → Database
- Server → External APIs

Validate all data crossing these boundaries.

---

# HTTP Essentials

## Common Methods

| Method | Purpose | Security Risk |
|---------|----------|---------------|
| GET | Read data | Information disclosure |
| POST | Submit data | Injection |
| PUT | Update resources | Unauthorized modification |
| PATCH | Partial update | Authorization flaws |
| DELETE | Remove resources | Privilege abuse |

---

## Important Headers

| Header | Security Importance |
|---------|---------------------|
| Authorization | Authentication tokens |
| Cookie | Session management |
| Host | Host Header Injection |
| Referer | Information leakage |
| User-Agent | Fingerprinting |
| X-Forwarded-For | IP spoofing risks |

---

# Authentication vs Authorization

Authentication

- Who are you?

Authorization

- What are you allowed to do?

Never assume authenticated users are authorized.

---

# Cookies & Sessions

Cookie

- Stored in browser

Session

- Stored on server

Important Cookie Flags

- Secure
- HttpOnly
- SameSite

Common Risks

- Session hijacking
- Session fixation
- Cookie theft

---

# SOP & CORS

## SOP

Browser security boundary.

Blocks JavaScript from reading data across origins.

---

## CORS

Controlled exception to SOP.

Common Risks

- Wildcard origins
- Reflected Origin
- Credentials with untrusted origins
- Overly permissive policies

---

# APIs

Common Types

- REST
- GraphQL
- SOAP

High-Risk Areas

- Authentication
- Authorization
- Tokens
- Rate limiting
- Parameter tampering

Common API Issues

- Broken Authentication
- Broken Authorization (BOLA)
- Excessive Data Exposure
- Mass Assignment
- Injection
- Missing Rate Limiting

---

# High-Risk Components

- Login systems
- Registration
- Password reset
- Session handling
- File uploads
- Search functionality
- Administrative panels
- APIs
- Business logic

---

# Common Vulnerabilities

## Injection

- SQL Injection
- Command Injection
- NoSQL Injection

---

## Access Control

- IDOR
- Privilege Escalation
- Missing Authorization
- Forced Browsing

---

## Client-Side

- Cross-Site Scripting (XSS)
- Cross-Site Request Forgery (CSRF)
- CORS Misconfiguration

---

## Server-Side

- Server-Side Request Forgery (SSRF)
- Local File Inclusion (LFI)
- Remote File Inclusion (RFI)
- File Upload Vulnerabilities
- Security Misconfiguration

---

## Business Logic

- Workflow bypass
- Race conditions
- Price manipulation
- Abuse of application functionality

---

# Input Handling

Recommended

- Allowlist validation
- Server-side validation
- Output encoding
- Context-aware escaping
- Parameterized queries

Avoid

- Trusting client input
- Client-only validation
- String concatenation in queries

---

# Typical Web Testing Flow

```
Recon

↓

Map Application

↓

Identify Entry Points

↓

Test Input Validation

↓

Test Authentication

↓

Test Authorization

↓

Test Session Management

↓

Test Business Logic

↓

Chain Vulnerabilities

↓

Assess Impact
```

---

# Common Attack Ideas

- Modify object IDs
- Change HTTP methods
- Remove parameters
- Add unexpected parameters
- Modify JSON values
- Replay requests
- Manipulate cookies
- Manipulate headers
- Enumerate endpoints
- Test hidden functionality

---

# Security Best Practices

- Validate all input.
- Enforce authorization on every request.
- Apply least privilege.
- Use HTTPS everywhere.
- Protect session cookies.
- Implement rate limiting.
- Log security events.
- Keep components updated.
- Return only necessary data.
- Never trust client-side controls.

---

# Pentester Mindset

When testing a web application, ask:

- What input reaches the backend?
- Where does trust change?
- Can authentication be bypassed?
- Can authorization be abused?
- What assumptions does the application make?
- Can multiple weaknesses be chained together?

---

# Quick Review

## Input

- Never trusted

## Authentication

- Verify identity

## Authorization

- Verify permissions

## Sessions

- Protect cookies

## APIs

- Protect every endpoint

## CORS

- Trust only approved origins

## Server

- Validate everything

## Database

- Use parameterized queries

---

# Key Insight

> Modern web applications are built on trust boundaries. Successful attackers rarely exploit a single vulnerability they chain multiple weaknesses across authentication, authorization, input handling, sessions, and APIs until trust completely breaks down.
