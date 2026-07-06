# HTTP Methods & Headers (Security Perspective)

## Introduction

HTTP methods and headers define how clients and servers communicate during every web request.

From a security perspective:

> Every HTTP request is a collection of user-controlled information that may influence application behavior, authentication, authorization, caching, routing, and logging.

Understanding how HTTP methods and headers work is essential because many web vulnerabilities originate from improper request handling rather than flaws in the underlying protocol.

---

# HTTP Request Structure

A typical HTTP request consists of:

- Request Method
- Target URL
- HTTP Version
- Headers
- Optional Request Body

Example:

```
POST /login HTTP/1.1
Host: example.com
Cookie: session=abc123
Content-Type: application/json

{
    "username":"alice",
    "password":"password"
}
```

Every part of this request may become an attack vector.

---

# HTTP Methods

## GET

Purpose:

- Retrieve resources

Characteristics:

- Safe
- Idempotent
- Usually cacheable

Security considerations:

- Sensitive information in URLs
- Information disclosure
- Parameter manipulation
- Forced browsing

---

## POST

Purpose:

- Submit data to the server

Common uses:

- Login forms
- Registration
- API requests
- File uploads

Security considerations:

- Input validation
- CSRF
- SQL Injection
- Business logic abuse

---

## PUT

Purpose:

- Replace an existing resource

Security considerations:

- Unauthorized modification
- File upload abuse
- Missing authorization checks

---

## PATCH

Purpose:

- Partially update a resource

Security considerations:

- Parameter tampering
- Broken access control

---

## DELETE

Purpose:

- Remove resources

Security considerations:

- Broken authorization
- Object ownership validation
- IDOR vulnerabilities

---

## OPTIONS

Purpose:

- Discover supported HTTP methods

Security considerations:

- Information disclosure
- CORS enumeration

---

## HEAD

Purpose:

- Retrieve headers only

Security considerations:

- Resource enumeration
- Cache behavior analysis

---

# HTTP Headers

Headers provide metadata about requests and responses.

Although they appear simple, many influence authentication, routing, caching, and security decisions.

---

## Authorization

Purpose:

Carries authentication credentials.

Examples:

- Bearer Tokens
- Basic Authentication
- API Keys

Security risks:

- Token leakage
- Missing validation
- Token reuse

---

## Cookie

Purpose:

Stores session identifiers and application state.

Security risks:

- Session hijacking
- Session fixation
- Missing Secure flag
- Missing HttpOnly flag
- Missing SameSite attribute

---

## Host

Purpose:

Identifies the destination host.

Security risks:

- Host Header Injection
- Password reset poisoning
- Cache poisoning
- SSRF assistance

---

## Referer

Purpose:

Indicates the previous page.

Security risks:

- Sensitive information leakage
- Internal URL disclosure

---

## User-Agent

Purpose:

Identifies the client software.

Security risks:

- Fingerprinting
- Filter bypass
- Log poisoning

---

## X-Forwarded-For

Purpose:

Communicates the original client IP when proxies are used.

Security risks:

- IP spoofing
- Authentication bypass
- Rate limit bypass
- Logging manipulation

---

## Origin

Purpose:

Identifies the origin of cross-origin requests.

Security risks:

- CORS misconfiguration
- CSRF validation bypass

---

## Content-Type

Purpose:

Describes the format of transmitted data.

Security risks:

- Content-Type confusion
- Deserialization attacks
- File upload bypasses

---

## Accept

Purpose:

Specifies acceptable response formats.

Security considerations:

- API content negotiation
- Unexpected parser behavior

---

# Common Security Misconfigurations

Pentesters frequently discover:

- Dangerous HTTP methods enabled
- Missing security headers
- Host header trust
- Weak cookie attributes
- Improper proxy configuration
- Sensitive headers disclosed
- Inconsistent authentication between methods

---

# Pentester Perspective

During an assessment, HTTP methods and headers are manipulated to identify:

- Hidden functionality
- Authentication weaknesses
- Authorization bypasses
- Session handling flaws
- Reverse proxy misconfigurations
- Cache poisoning opportunities
- Request smuggling prerequisites
- Business logic flaws

Rather than treating headers as metadata, pentesters consider them user-controlled input that may alter application behavior.

---

# Key Takeaways

- HTTP methods define the requested action.
- HTTP headers provide contextual information that often influences server-side decisions.
- Every request component should be considered untrusted input.
- Incorrect handling of methods or headers frequently results in authentication, authorization, or routing vulnerabilities.

---

# Key Insight

> Successful web attacks rarely depend on manipulating a single parameter. They often begin by understanding how HTTP methods, headers, and server-side logic interact to process every request.
