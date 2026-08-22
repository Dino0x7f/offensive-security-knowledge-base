# Reflected Cross-Site Scripting (Reflected XSS)

## Overview

Reflected Cross-Site Scripting (Reflected XSS) is a client-side vulnerability that occurs when an application immediately reflects untrusted user input in an HTTP response without properly validating or encoding it. As a result, attacker-controlled JavaScript executes in the victim's browser within the security context of the vulnerable website.

Unlike Stored XSS, the malicious payload is **not permanently stored** on the server. Instead, it is delivered through a specially crafted request, making the victim's interaction with the malicious link or request necessary for exploitation.

Reflected XSS is one of the most common web application vulnerabilities and remains a frequent finding in penetration tests.

---

# How Reflected XSS Works

Typical execution flow:

```
Attacker

↓

Crafts Malicious URL

↓

Victim Opens Link

↓

Application Reflects Input

↓

Browser Renders Response

↓

JavaScript Executes
```

The payload exists only for the duration of the request.

---

# Root Cause

Reflected XSS occurs because applications insert user-controlled input into HTML responses without proper output encoding.

Common causes include:

- Search functionality
- Error messages
- Login forms
- Redirect pages
- HTTP parameters
- URL fragments
- Dynamic page generation

---

# Attack Surface

Reflected XSS commonly appears in:

- Search pages
- Login portals
- Error pages
- Contact forms
- URL parameters
- Filter pages
- Product searches
- Redirect functionality

Any page that immediately reflects user input back to the browser may be vulnerable.

---

# Common Reflection Contexts

The severity and exploitation method depend on where the input is reflected.

### HTML Context

Input appears within page content.

---

### HTML Attribute Context

Input appears inside HTML attributes.

---

### JavaScript Context

Input is inserted into JavaScript code.

---

### CSS Context

Input is reflected inside style blocks.

---

### URL Context

Input is reflected within hyperlinks or redirects.

Each context requires different encoding and defensive measures.

---

# Types of Reflected XSS

## Direct Reflection

The payload is immediately reflected into the response.

---

## DOM-Assisted Reflection

The server reflects input, and client-side JavaScript subsequently processes it.

---

## Filter Bypass Reflection

The application attempts to sanitize input but can be bypassed due to incomplete filtering.

---

# Potential Impact

Successful exploitation may allow attackers to:

- Execute arbitrary JavaScript
- Steal session cookies (where accessible)
- Hijack authenticated sessions
- Perform actions as the victim
- Modify page content
- Display phishing forms
- Capture user input
- Redirect users to malicious websites
- Access browser storage
- Interact with application APIs using the victim's session

The impact depends on browser protections, application security controls, and available client-side data.

---

# Common Indicators

Possible indicators include:

- User input appears unchanged in responses
- Reflected parameters inside HTML
- JavaScript execution after clicking a URL
- Unexpected browser pop-ups during testing
- Browser developer tools showing reflected input
- Security scanner alerts

---

# Exploitation Requirements

Successful exploitation typically requires:

- A vulnerable reflection point
- Victim interaction (opening a link or submitting a request)
- Executable JavaScript context
- Absence of effective browser or application defenses

Unlike Stored XSS, persistent storage of the payload is not required.

---

# Mitigation

Recommended defenses include:

- Context-aware output encoding
- Input validation
- HTML escaping
- JavaScript escaping
- Attribute encoding
- Content Security Policy (CSP)
- Secure template engines
- Avoid directly reflecting user input

Output encoding should match the context in which the data is rendered.

---

# Detection Methods

Security professionals identify Reflected XSS through:

- Manual testing
- Source code review
- Dynamic Application Security Testing (DAST)
- Browser developer tools
- Automated vulnerability scanners
- Fuzzing reflection points

Testing should cover all user-controlled input sources.

---

# Reflected XSS vs Stored XSS

| Reflected XSS | Stored XSS |
|---------------|------------|
| Payload is reflected immediately | Payload is permanently stored |
| Requires victim interaction | Executes whenever affected content is viewed |
| Exists only in a single request | Persists until removed |
| Common in search and error pages | Common in comments, profiles, and forums |

---

# Relationship to Other Vulnerabilities

Reflected XSS may be combined with:

```
Reflected XSS

↓

Session Hijacking

↓

Credential Theft

↓

Account Takeover

↓

Privilege Abuse

↓

Application Compromise
```

Although often considered less severe than Stored XSS, successful social engineering can make Reflected XSS highly impactful.

---

# Real-World Examples

Reflected XSS has historically affected:

- Search engines
- Webmail interfaces
- Online banking portals
- E-commerce platforms
- Government websites
- Authentication portals
- Enterprise applications

It continues to appear in modern applications due to improper output handling.

---

# Importance in Offensive Security

Understanding Reflected XSS enables penetration testers to:

- Identify unsafe output rendering
- Assess client-side security
- Evaluate input handling
- Test browser-side attack vectors
- Demonstrate session compromise risks
- Recommend secure output encoding practices

---

> **Key Insight:** Reflected XSS occurs when applications immediately return untrusted input to the browser without proper output encoding. The vulnerability exists because the browser interprets attacker-controlled data as executable code rather than displaying it as harmless content.