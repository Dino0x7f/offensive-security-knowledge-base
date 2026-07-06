# Web Security Basics Overview

## Introduction

Web security is the discipline of protecting web applications, APIs, and web services from attacks that exploit implementation flaws, insecure configurations, broken business logic, and weaknesses in communication between system components.

From a security perspective:

> Every web application is an interface that accepts untrusted input, processes it, and interacts with valuable assets such as databases, files, authentication systems, and third-party services.

Understanding how web applications operate is the foundation of modern penetration testing, as most external assessments begin with web-based targets.

---

# Why Web Security Matters

Modern organizations rely heavily on web technologies for:

- Business applications
- Customer portals
- Administrative dashboards
- REST and GraphQL APIs
- Cloud services
- Authentication platforms

Because these applications are typically accessible over the Internet, they represent one of the largest and most frequently targeted attack surfaces.

Compromising a single web application can provide access to sensitive data, internal infrastructure, or enterprise identities.

---

# High-Level Web Application Architecture

A typical web application consists of several interacting components:

```
Client (Browser)

↓

Web Server

↓

Application Logic

↓

Database

↓

External Services / APIs
```

Each component introduces its own trust boundaries, attack surface, and potential security weaknesses.

---

# Primary Attack Surface

During an assessment, attackers commonly analyze:

- HTTP requests and responses
- URL parameters
- Form inputs
- Cookies
- HTTP headers
- File upload functionality
- Authentication mechanisms
- Authorization controls
- Session management
- API endpoints
- Search functionality
- Administrative interfaces

Every location where user-controlled data enters the application should be considered part of the attack surface.

---

# Fundamental Security Principles

## Never Trust User Input

All input should be considered malicious until validated.

Input validation should occur on the server regardless of any client-side restrictions.

---

## Authentication vs Authorization

Authentication answers:

> Who are you?

Authorization answers:

> What are you allowed to do?

Strong authentication does not guarantee correct authorization.

---

## Server-Side Security Is Authoritative

Client-side controls exist primarily for usability.

Attackers can:

- Modify requests
- Remove JavaScript validation
- Forge HTTP requests
- Manipulate cookies
- Interact directly with APIs

Security decisions must always be enforced on the server.

---

## Defense in Depth

No single security mechanism is sufficient.

Effective web security combines:

- Secure coding
- Authentication
- Authorization
- Input validation
- Logging
- Monitoring
- Encryption
- Secure configuration

---

# Common Security Objectives

Web applications aim to ensure:

- Confidentiality of sensitive information
- Integrity of stored and transmitted data
- Availability of services
- Proper authentication
- Correct authorization
- Accountability through logging and auditing

---

# Why Web Applications Become Vulnerable

Most vulnerabilities arise from:

- Insufficient input validation
- Broken access control
- Authentication weaknesses
- Session management flaws
- Business logic errors
- Insecure file handling
- Misconfigured servers
- Unsafe framework configurations
- Insecure third-party integrations

These weaknesses often result from implementation mistakes rather than flaws in the underlying technologies.

---

# Pentester Perspective

A penetration tester evaluates a web application by identifying:

- Input entry points
- Trust boundaries
- Authentication mechanisms
- Authorization logic
- Sensitive functionality
- Business workflows
- Hidden endpoints
- API behavior
- Data exposure
- Security misconfigurations

Rather than asking:

> "Does the application work?"

The pentester asks:

> "How can the application be manipulated to behave in ways the developers never intended?"

---

# Key Takeaways

- Web applications process untrusted input and expose valuable assets.
- Every user-controlled input is a potential attack vector.
- Authentication and authorization solve different security problems.
- Client-side controls cannot be trusted.
- Most successful web attacks exploit implementation flaws rather than protocol weaknesses.

---

# Key Insight

> Web security is fundamentally about controlling how untrusted input influences application behavior, business logic, and access to sensitive resources.
