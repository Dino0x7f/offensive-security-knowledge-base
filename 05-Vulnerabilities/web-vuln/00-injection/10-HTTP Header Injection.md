# HTTP Header Injection

## Overview

HTTP Header Injection is a vulnerability that occurs when an application includes untrusted user input in HTTP response or request headers without proper validation or encoding. An attacker can manipulate existing headers or inject new ones, potentially altering the behavior of browsers, proxies, caches, or downstream applications.

HTTP headers define how clients and servers communicate. When attackers gain control over these headers, they may influence security mechanisms, session management, caching behavior, redirects, or content interpretation.

HTTP Header Injection often results from **CRLF Injection**, but it can also arise from insecure application logic that allows direct manipulation of header values.

---

# What are HTTP Headers?

HTTP headers are metadata exchanged between clients and servers.

Example:

```
GET / HTTP/1.1

Host: example.com

User-Agent: Browser

Accept: text/html
```

Server response:

```
HTTP/1.1 200 OK

Content-Type: text/html

Set-Cookie: session=abc123

Cache-Control: no-cache
```

Headers control communication, security, caching, authentication, and content handling.

---

# How HTTP Header Injection Works

Typical flow:

```
User Input

↓

Application

↓

HTTP Header

↓

Web Server

↓

Client
```

If user-controlled data becomes part of a header, attackers may manipulate its value or inject additional headers.

---

## Example

Application generates:

```
Location: /profile?user=alice
```

If user input controls the value without validation, an attacker may modify the resulting header or influence the behavior of clients and intermediaries.

---

# Root Cause

HTTP Header Injection occurs because applications trust user input when generating HTTP headers.

Common causes include:

- Dynamic header generation
- Unsafe redirects
- Custom authentication headers
- Cookie creation
- Missing CRLF filtering
- Insecure proxy integration
- Improper framework usage

---

# Attack Surface

HTTP Header Injection commonly appears in:

- Redirect functionality
- Login systems
- Session management
- Custom response headers
- API gateways
- Reverse proxies
- Load balancers
- File download features

---

# Common Targets

Applications generating:

- Location
- Set-Cookie
- Refresh
- Content-Disposition
- Content-Type
- Cache-Control
- X-Forwarded-* headers
- Custom security headers

---

# Types of HTTP Header Injection

## Response Header Injection

Attackers inject or manipulate server response headers.

---

## Request Header Injection

Applications trust attacker-controlled request headers.

Examples include:

- X-Forwarded-For
- Host
- Referer
- Origin

---

## Cookie Manipulation

Injected headers alter session cookies.

Possible consequences:

- Session Fixation
- Cookie Poisoning
- Security Attribute Removal

---

## Cache Manipulation

Attackers influence caching behavior by modifying cache-related headers.

---

## Security Header Manipulation

Headers controlling browser security may be altered.

Examples include:

- Content-Security-Policy
- X-Frame-Options
- Strict-Transport-Security
- X-Content-Type-Options

---

# Potential Impact

Successful exploitation may allow attackers to:

- Modify HTTP responses
- Manipulate cookies
- Influence browser behavior
- Poison shared caches
- Bypass security policies
- Enable phishing attacks
- Support Cross-Site Scripting
- Facilitate Session Hijacking

The impact depends on which headers are affected.

---

# Common Indicators

Possible indicators include:

- Unexpected response headers
- Duplicate HTTP headers
- Invalid redirects
- Cookie anomalies
- Proxy inconsistencies
- Cache irregularities

---

# Mitigation

Recommended defenses include:

- Validate all header values
- Reject CR and LF characters
- Use framework-provided header APIs
- Avoid manual header construction
- Validate redirect destinations
- Restrict user-controlled header generation
- Apply secure default header policies

Applications should never allow external input to directly determine HTTP header structure or security-critical values.

---

# Detection Methods

Security professionals identify HTTP Header Injection through:

- Manual testing
- HTTP response inspection
- Proxy analysis
- Source code review
- Dynamic application testing
- Automated vulnerability scanners

---

# HTTP Header Injection vs CRLF Injection

| HTTP Header Injection | CRLF Injection |
|-----------------------|----------------|
| Manipulates HTTP headers | Injects protocol line breaks |
| May occur with or without CRLF | Primitive used to inject new headers |
| Targets header values | Targets protocol formatting |

CRLF Injection is often the mechanism that enables HTTP Header Injection, but applications may also expose header injection through insecure APIs.

---

# Relationship to Other Vulnerabilities

HTTP Header Injection frequently enables additional attacks.

```
HTTP Header Injection

↓

Cookie Manipulation

↓

Response Splitting

↓

Cache Poisoning

↓

Cross-Site Scripting

↓

Session Compromise
```

It is commonly chained with other web vulnerabilities to increase impact.

---

# Real-World Examples

HTTP Header Injection has been identified in:

- Web Frameworks
- Reverse Proxies
- Authentication Gateways
- API Platforms
- Content Management Systems
- Enterprise Web Applications

Many historical cache poisoning and response splitting vulnerabilities began with insecure HTTP header generation.

---

# Importance in Offensive Security

Understanding HTTP Header Injection enables penetration testers to:

- Assess HTTP response generation
- Evaluate session management
- Test security header implementation
- Analyze cache behavior
- Identify proxy trust issues
- Recommend secure HTTP header handling

---

> **Key Insight:** HTTP headers are a critical part of the web security model. When applications allow attackers to manipulate header values or structure, they can influence browser behavior, session management, caching, and security controls, often enabling more complex attacks across the entire HTTP communication chain.