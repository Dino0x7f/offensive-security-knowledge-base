# Stored Cross-Site Scripting (Stored XSS)

## Overview

Stored Cross-Site Scripting (Stored XSS), also known as **Persistent XSS**, is a client-side vulnerability that occurs when an application stores untrusted user input and later renders it to other users without proper output encoding.

Unlike Reflected XSS, the malicious payload is permanently stored by the application—typically in a database, file system, or other persistent storage—and executes every time the affected content is viewed.

Because the attacker does not need to directly interact with every victim after the payload is stored, Stored XSS is generally considered more dangerous than Reflected XSS.

---

# How Stored XSS Works

Typical execution flow:

```
Attacker

↓

Submits Malicious Input

↓

Application Stores Payload

↓

Victim Requests Page

↓

Application Returns Stored Data

↓

Browser Executes JavaScript
```

The payload remains active until it is removed or properly sanitized.

---

# Root Cause

Stored XSS occurs because applications store user-controlled input and later render it without applying context-aware output encoding.

Common causes include:

- Missing output encoding
- Trusting stored user content
- Unsafe HTML rendering
- Rich text editors
- Markdown rendering
- Improper template handling

The vulnerability arises during **output**, not during storage.

---

# Attack Surface

Stored XSS commonly appears in features where user-generated content is saved.

Examples include:

- Comments
- Forum posts
- User profiles
- Chat messages
- Support tickets
- Product reviews
- Blog posts
- Wiki pages
- Administrative notes
- Notification systems

Any persistent user-controlled content may become an attack vector.

---

# Common Storage Locations

Stored payloads may reside in:

- Databases
- File systems
- CMS content
- Log viewers
- Message queues
- Cached objects
- Search indexes

The storage medium is less important than the application's later rendering behavior.

---

# Types of Stored XSS

## Stored HTML Injection

Malicious HTML is stored and rendered.

---

## Stored JavaScript Injection

JavaScript executes when the page is loaded.

---

## Stored DOM-Assisted XSS

Stored data is processed by client-side JavaScript, resulting in execution.

---

## Second-Order XSS

The payload is stored safely but becomes dangerous when another application or process later renders it insecurely.

---

# Potential Impact

Successful exploitation may allow attackers to:

- Execute arbitrary JavaScript
- Steal session information (where accessible)
- Hijack authenticated sessions
- Perform actions as victims
- Modify page content
- Display phishing interfaces
- Capture user credentials
- Access browser storage
- Interact with APIs using victim credentials
- Target privileged users such as administrators

Because every visitor may execute the payload, Stored XSS often has a significantly larger impact than Reflected XSS.

---

# Common Indicators

Possible indicators include:

- Persistent malicious content
- JavaScript execution after page reload
- Multiple users affected
- Unexpected browser behavior
- Administrator account compromise
- Security scanner findings

---

# Exploitation Requirements

Successful exploitation generally requires:

- A persistent storage location
- Rendering of stored content
- Executable browser context
- Lack of effective output encoding

Victim interaction beyond viewing the affected content is typically unnecessary.

---

# Mitigation

Recommended defenses include:

- Context-aware output encoding
- HTML sanitization
- Safe Markdown rendering
- Secure rich text editors
- Content Security Policy (CSP)
- Trusted template engines
- Validate allowed HTML elements where rich content is required
- Never trust stored data simply because it originates from the application database

Stored data should always be treated as untrusted when rendered.

---

# Detection Methods

Security professionals identify Stored XSS through:

- Manual testing
- Source code review
- Dynamic Application Security Testing (DAST)
- Stored input analysis
- Browser developer tools
- Automated vulnerability scanners

Testing should include all features that store user-generated content.

---

# Stored XSS vs Reflected XSS

| Stored XSS | Reflected XSS |
|------------|---------------|
| Payload is stored permanently | Payload exists only in the current request |
| Executes whenever content is viewed | Requires victim to trigger a crafted request |
| Affects multiple users | Usually targets individual victims |
| Often more severe | Typically less scalable |

Stored XSS generally presents a higher business risk because it can compromise every user who accesses the affected resource.

---

# Relationship to Other Vulnerabilities

Stored XSS frequently serves as the starting point for broader attacks.

```
Stored XSS

↓

Administrator Visits Page

↓

Session Compromise

↓

Privilege Abuse

↓

Administrative Access

↓

Application Compromise
```

Attackers often target administrative interfaces because administrators naturally review user-generated content.

---

# Real-World Examples

Stored XSS has been identified in:

- Social media platforms
- Discussion forums
- Customer support portals
- Content Management Systems (CMS)
- Project management tools
- Enterprise collaboration platforms
- E-commerce review systems

Historically, many major web applications have experienced Stored XSS vulnerabilities due to improper handling of user-generated content.

---

# Importance in Offensive Security

Understanding Stored XSS enables penetration testers to:

- Assess user-generated content handling
- Evaluate persistent input processing
- Identify unsafe rendering mechanisms
- Test administrator-facing functionality
- Demonstrate client-side privilege escalation
- Recommend secure content rendering practices

---

> **Key Insight:** Stored XSS is dangerous because malicious content becomes part of the application's normal data. Every time that content is rendered without proper output encoding, the browser treats attacker-controlled input as executable code, potentially compromising every user who views it.