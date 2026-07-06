# Same-Origin Policy (SOP) and CORS (Security Perspective)

## Overview

Modern web applications constantly communicate across multiple domains, APIs, CDNs, and third-party services.

To prevent websites from freely accessing data belonging to other websites, browsers enforce the **Same-Origin Policy (SOP)**.

When legitimate cross-origin communication is required, servers can selectively relax these restrictions using **Cross-Origin Resource Sharing (CORS)**.

From a security perspective:

> SOP establishes the browser's trust boundary, while CORS defines controlled exceptions to that boundary.

---

# Understanding an Origin

An **Origin** is defined by three components:

- Protocol (HTTP / HTTPS)
- Host (Domain or IP)
- Port

Example:

| URL | Origin |
|------|--------|
| https://example.com | HTTPS + example.com + 443 |
| http://example.com | Different Origin |
| https://api.example.com | Different Origin |
| https://example.com:8443 | Different Origin |

Changing **any** of these components creates a new origin.

---

# Same-Origin Policy (SOP)

## What is SOP?

Same-Origin Policy is a browser security mechanism that restricts JavaScript running on one origin from reading data belonging to another origin.

It protects users from malicious websites attempting to access sensitive information from trusted websites.

---

# What SOP Protects

Without SOP, a malicious website could potentially:

- Read online banking data
- Access webmail content
- Steal authentication responses
- Retrieve private API responses

SOP prevents these scenarios by isolating origins from one another.

---

# What SOP Does NOT Prevent

SOP does **not** prevent:

- Sending requests to another website
- Redirecting users
- Loading external images
- Loading JavaScript libraries
- Loading CSS resources

Instead, SOP prevents malicious JavaScript from reading sensitive responses.

---

# Cross-Origin Resource Sharing (CORS)

## What is CORS?

CORS is an extension to SOP that allows servers to explicitly specify which external origins are allowed to access their resources.

Instead of removing SOP, CORS creates controlled exceptions.

---

# How CORS Works

When a browser sends a cross-origin request:

1. Browser detects cross-origin communication.
2. Server responds with CORS headers.
3. Browser evaluates the policy.
4. Browser decides whether JavaScript may access the response.

The browser—not the server—enforces the CORS policy.

---

# Important CORS Headers

## Access-Control-Allow-Origin

Specifies which origin is allowed to access the resource.

Example values:

- https://example.com
- https://app.example.com

---

## Access-Control-Allow-Credentials

Determines whether cookies or authentication credentials may be included in cross-origin requests.

---

## Access-Control-Allow-Methods

Defines which HTTP methods are permitted.

Examples:

- GET
- POST
- PUT
- DELETE

---

## Access-Control-Allow-Headers

Specifies which custom headers clients may send.

Examples:

- Authorization
- Content-Type
- X-API-Key

---

# Preflight Requests

Certain cross-origin requests require browser verification before the actual request.

The browser automatically sends an **OPTIONS** request to ask:

- Is this origin allowed?
- Is this HTTP method allowed?
- Are these headers permitted?

Only if approved does the browser send the real request.

---

# Common CORS Misconfigurations

## 1. Wildcard Origin

```
Access-Control-Allow-Origin: *
```

Allows every origin to access public resources.

Generally unsafe for authenticated APIs.

---

## 2. Reflecting Arbitrary Origins

The server blindly reflects any Origin header received.

Example:

```
Origin: https://evil.com
```

Response:

```
Access-Control-Allow-Origin: https://evil.com
```

This effectively trusts any website.

---

## 3. Allowing Credentials with Untrusted Origins

Using:

```
Access-Control-Allow-Credentials: true
```

together with overly permissive origins can expose authenticated user data.

---

## 4. Overly Broad Trust Relationships

Allowing:

- Development domains
- Temporary environments
- Third-party domains
- Forgotten subdomains

These often become attack paths.

---

# Security Risks

Poor CORS configuration can lead to:

- Sensitive data disclosure
- API abuse
- Authenticated data leakage
- Cross-origin information theft
- Broken trust boundaries

---

# Attacker Perspective

Attackers analyze CORS by asking:

- Which origins are trusted?
- Are credentials allowed?
- Can arbitrary origins be reflected?
- Can authenticated API responses be read?
- Are development domains still trusted?

The objective is to determine whether browser protections can be bypassed through server misconfiguration.

---

# Relationship Between SOP and CORS

| SOP | CORS |
|------|------|
| Browser security policy | Controlled exception mechanism |
| Blocks unauthorized cross-origin access | Allows approved cross-origin access |
| Enabled by default | Configured by the server |
| Protects user data | Must be carefully implemented |

---

# Security Best Practices

- Allow only trusted origins.
- Avoid wildcard origins for sensitive resources.
- Never trust user-controlled Origin headers blindly.
- Restrict credentialed requests to explicitly approved domains.
- Regularly review trusted origins.
- Separate public APIs from authenticated APIs.

---

# Key Takeaways

- SOP is one of the browser's most important security mechanisms.
- CORS does not replace SOP—it extends it safely.
- Browsers enforce CORS, not servers.
- Most CORS vulnerabilities result from overly permissive trust relationships.
- Cross-origin access should always follow the principle of least privilege.

---

# Key Insight

> SOP establishes the browser's security boundary. CORS intentionally opens controlled doors through that boundary and every unnecessary door becomes a potential attack path.
