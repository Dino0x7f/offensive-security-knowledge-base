# Web Architecture Basics (Security Perspective)

## Overview

Web architecture describes how the different components of a web application interact to process requests, execute business logic, store data, and return responses.

From a security perspective:

> Every component in the architecture introduces a trust boundary, an attack surface, and potential security weaknesses.

---

# Why Web Architecture Matters

Understanding architecture allows security professionals to:

- Identify attack surfaces
- Trace request flow
- Locate trust boundaries
- Understand privilege separation
- Discover security assumptions

Without understanding architecture, web vulnerabilities become isolated concepts instead of complete attack chains.

---

# High-Level Web Architecture

A modern web application typically consists of:

```
Browser
      │
      ▼
Load Balancer
      │
      ▼
Web Server
      │
      ▼
Application Server
      │
      ▼
Database
      │
      ▼
Storage / External APIs / Internal Services
```

Each layer processes user-controlled input before passing it to the next layer.

---

# Core Components

## 1. Client

Usually:

- Browser
- Mobile application
- Desktop application

Responsibilities:

- Display UI
- Collect user input
- Send HTTP requests

Security Perspective:

Client-side controls cannot be trusted because attackers fully control the client.

---

## 2. Web Server

Examples:

- Nginx
- Apache
- IIS

Responsibilities:

- Receive HTTP requests
- Serve static content
- Forward dynamic requests

Security Risks:

- Directory listing
- Information disclosure
- Weak TLS configuration
- Misconfigured virtual hosts

---

## 3. Application Server

Runs business logic.

Examples:

- PHP
- Node.js
- Java
- ASP.NET
- Python (Django / Flask)

Responsibilities:

- Authentication
- Authorization
- Input processing
- Database interaction
- Session management

Security Risks:

- SQL Injection
- XSS
- SSRF
- IDOR
- Authentication flaws
- Logic vulnerabilities

---

## 4. Database

Stores persistent application data.

Examples:

- MySQL
- PostgreSQL
- MSSQL
- MongoDB

Stores:

- Users
- Password hashes
- Sessions
- Orders
- Financial data

Security Risks:

- SQL Injection
- Weak permissions
- Exposed databases
- Sensitive data leakage

---

## 5. External Services

Modern applications communicate with:

- Payment providers
- Email services
- Authentication providers
- Cloud storage
- Third-party APIs

Security Risks:

- API abuse
- SSRF
- Trust boundary failures
- Token leakage

---

# Request Flow

A typical request follows this path:

```
User

↓

Browser

↓

HTTP Request

↓

Web Server

↓

Application Logic

↓

Database

↓

Application Logic

↓

HTTP Response

↓

Browser
```

Every stage processes attacker-controlled input.

---

# Trust Boundaries

A trust boundary exists whenever data moves between components with different trust levels.

Examples:

```
Browser → Server

Server → Database

Application → External API

Internal Service → Cloud Service
```

Crossing a trust boundary always requires validation.

---

# Common Architectural Patterns

## Monolithic Applications

Single application containing:

- UI
- Business logic
- Database interaction

Advantages:

- Simple deployment
- Easy development

Security Challenges:

- Large attack surface
- Full compromise affects entire application

---

## Three-Tier Architecture

Layers:

- Presentation
- Application
- Database

Benefits:

- Separation of responsibilities
- Better security controls
- Easier access management

---

## Microservices

Application divided into multiple services.

Benefits:

- Scalability
- Independent deployment

Security Challenges:

- API authentication
- Service trust
- Internal communication security
- Token management

---

# Security Controls Across Architecture

Each layer should implement its own controls.

| Layer | Security Control |
|--------|------------------|
| Client | Input restrictions (UX only) |
| Web Server | TLS, WAF, Rate Limiting |
| Application | Authentication, Authorization, Validation |
| Database | Least privilege, Encryption |
| Infrastructure | Firewalls, Monitoring, Segmentation |

No single layer should be trusted alone.

---

# Common Architectural Weaknesses

- Missing trust boundaries
- Weak authentication
- Broken authorization
- Direct database exposure
- Hardcoded secrets
- Poor API design
- Excessive privileges
- Lack of logging
- Missing rate limiting

---

# Attacker Perspective

Attackers don't see:

- Pages
- Buttons
- Menus

They see:

- Components
- Request flow
- Data flow
- Trust relationships
- Authentication points
- Authorization decisions

They ask:

- Where does user input reach?
- Where is validation missing?
- Where does trust change?
- Which component has the highest privilege?

---

# Pentester Mindset

When assessing a web application, think in terms of architecture:

1. Map all components.
2. Identify trust boundaries.
3. Trace data flow.
4. Locate authentication points.
5. Analyze authorization decisions.
6. Find external integrations.
7. Look for opportunities to chain weaknesses.

---

# Key Takeaways

- Every architecture layer introduces a new attack surface.
- User input flows through multiple trusted components.
- Trust boundaries are where validation must occur.
- Modern attacks usually exploit interactions between components rather than isolated systems.
- Understanding architecture makes vulnerability discovery significantly easier.

---

# Key Insight

> A web application is not a single system it is a collection of interconnected components. Successful attackers exploit the relationships between those components rather than attacking them in isolation.
