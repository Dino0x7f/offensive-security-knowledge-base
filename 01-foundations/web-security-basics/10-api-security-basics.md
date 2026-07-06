# API Security Basics (Security Perspective)

## Overview

An **Application Programming Interface (API)** enables software components to communicate and exchange data.

Modern web applications, mobile apps, cloud platforms, and microservices all rely heavily on APIs.

From a security perspective:

> APIs expose backend functionality directly, making them one of the most valuable attack surfaces in modern applications.

---

# Why APIs Matter

Nearly every modern application uses APIs for:

- User authentication
- Data retrieval
- Payment processing
- File management
- Mobile applications
- Third-party integrations
- Cloud services

Unlike traditional web pages, APIs expose application logic directly to clients.

---

# Common API Types

## REST API

Most common API architecture.

Characteristics:

- HTTP-based
- Resource-oriented
- JSON responses
- Stateless communication

Example:

```
GET /api/users/15
```

---

## GraphQL

Allows clients to request exactly the data they need.

Advantages:

- Flexible queries
- Reduced over-fetching

Security Challenges:

- Complex authorization
- Query abuse
- Excessive resource consumption

---

## SOAP

XML-based protocol commonly found in legacy enterprise systems.

Security Considerations:

- XML parsing attacks
- Complex configurations
- Legacy authentication mechanisms

---

# Typical API Architecture

```
Client

↓

API Gateway

↓

Authentication

↓

Application Logic

↓

Database

↓

JSON Response
```

Every API request crosses multiple trust boundaries.

---

# Authentication in APIs

Authentication answers:

> Who is making this request?

Common mechanisms:

- API Keys
- Session Cookies
- JWT Tokens
- OAuth 2.0
- Bearer Tokens

Security Risks:

- Weak token generation
- Token leakage
- Missing authentication
- Long-lived credentials

---

# Authorization in APIs

Authorization answers:

> Is this user allowed to access this resource?

Example:

```
GET /api/users/15
```

The server must verify whether the authenticated user is permitted to access user **15**.

Failure to perform this check leads to Broken Access Control.

---

# Common API Security Risks

## 1. Broken Authentication

Weak authentication mechanisms allow attackers to impersonate legitimate users.

Examples:

- Predictable tokens
- Missing authentication
- Weak API keys

Impact:

- Account takeover
- Unauthorized access

---

## 2. Broken Authorization

The server authenticates the user but fails to verify permissions.

Examples:

- User ID manipulation
- Accessing another user's records

Impact:

- Data exposure
- Privilege escalation

---

## 3. Excessive Data Exposure

The API returns more information than necessary.

Example:

Instead of returning:

```
Name
Email
```

It also returns:

- Password hash
- Internal IDs
- Security settings

Impact:

- Information disclosure
- Easier attacker reconnaissance

---

## 4. Missing Rate Limiting

Unlimited requests allow attackers to automate attacks.

Examples:

- Password brute force
- Credential stuffing
- API enumeration

Impact:

- Account compromise
- Denial of Service

---

## 5. Injection Attacks

Untrusted API input reaches backend interpreters.

Examples:

- SQL Injection
- NoSQL Injection
- Command Injection

Impact:

- Data theft
- Remote code execution

---

## 6. Mass Assignment

The server automatically accepts unexpected client-supplied fields.

Example:

```
"isAdmin": true
```

If improperly handled, attackers may modify protected attributes.

Impact:

- Privilege escalation
- Business logic abuse

---

## 7. Security Misconfiguration

Examples:

- Debug endpoints enabled
- Default credentials
- Verbose error messages
- Public API documentation

Impact:

- Information leakage
- Easier exploitation

---

# API Attack Surface

Attackers examine:

- Endpoints
- Parameters
- HTTP methods
- Headers
- Authentication tokens
- JSON bodies
- File uploads
- Response data

Every exposed endpoint is a potential entry point.

---

# Common API Attack Scenarios

Examples include:

- Manipulating resource IDs to access other users' data
- Reusing stolen bearer tokens
- Modifying JSON parameters
- Enumerating hidden endpoints
- Bypassing authorization checks
- Abusing poorly protected administrative APIs

---

# Attacker Perspective

Attackers analyze APIs by asking:

- Which endpoints exist?
- What authentication is required?
- Can object identifiers be manipulated?
- Which parameters influence authorization?
- Is sensitive data returned unnecessarily?
- Are hidden administrative endpoints exposed?

Their objective is to interact directly with backend logic while bypassing frontend restrictions.

---

# API Security Best Practices

- Enforce strong authentication.
- Validate authorization on every request.
- Apply least privilege.
- Return only necessary data.
- Validate all input server-side.
- Implement rate limiting.
- Log suspicious API activity.
- Use HTTPS for all API communication.
- Rotate API credentials regularly.

---

# Key Takeaways

- APIs expose backend functionality directly.
- Authentication and authorization are separate security controls.
- Every endpoint increases the application's attack surface.
- Input validation remains critical even without a traditional web interface.
- Most API attacks exploit broken access control rather than software vulnerabilities.

---

# Key Insight

> APIs are not simply communication interfaces they are direct gateways to application logic. Securing an API means protecting every request, every resource, and every trust decision the backend makes.
