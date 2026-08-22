# DOM-Based Cross-Site Scripting (DOM XSS)

## Overview

DOM-Based Cross-Site Scripting (DOM XSS) is a client-side vulnerability that occurs when JavaScript running in the browser processes untrusted data and inserts it into the Document Object Model (DOM) in an unsafe manner, causing attacker-controlled code to execute.

Unlike Reflected XSS and Stored XSS, the vulnerable behavior exists entirely within the browser. The server may never include the malicious payload in its HTTP response. Instead, client-side JavaScript reads attacker-controlled input from the browser environment and injects it into the page.

DOM XSS has become increasingly common with the widespread adoption of modern JavaScript frameworks and Single Page Applications (SPAs).

---

# How DOM XSS Works

Typical execution flow:

```
Attacker

↓

Crafts Malicious URL

↓

Victim Opens Page

↓

JavaScript Reads Input

↓

Unsafe DOM Update

↓

Browser Executes JavaScript
```

The vulnerability exists in client-side JavaScript rather than in server-side rendering.

---

# Root Cause

DOM XSS occurs when JavaScript reads data from an untrusted source and writes it to a dangerous DOM sink without proper sanitization or encoding.

Common causes include:

- Unsafe DOM manipulation
- Dynamic HTML generation
- Client-side rendering
- Improper use of browser APIs
- Missing input validation
- Trusting URL parameters

---

# DOM Sources

A **source** is any location from which attacker-controlled data enters client-side JavaScript.

Common sources include:

- URL query parameters
- URL fragments (`#`)
- Document URL
- Referrer
- Cookies
- Local Storage
- Session Storage
- `window.name`
- Cross-window messaging
- Browser APIs

Any browser-controlled input should be considered untrusted.

---

# DOM Sinks

A **sink** is a JavaScript function or property that interprets attacker-controlled input.

Examples include operations that:

- Insert HTML into the DOM
- Execute JavaScript dynamically
- Modify browser navigation
- Update page content

Unsafe sinks may transform untrusted data into executable code.

---

# Attack Surface

DOM XSS commonly appears in:

- Single Page Applications (SPAs)
- Client-side routing
- Search functionality
- Dynamic dashboards
- JavaScript widgets
- Browser extensions
- Client-side templates
- Third-party JavaScript libraries

---

# Types of DOM XSS

## URL-Based DOM XSS

Payload originates from the URL.

---

## Hash-Based DOM XSS

Payload is extracted from the URL fragment.

---

## Storage-Based DOM XSS

Payload is loaded from browser storage.

---

## Message-Based DOM XSS

Payload originates from browser messaging mechanisms.

---

# Potential Impact

Successful exploitation may allow attackers to:

- Execute arbitrary JavaScript
- Hijack authenticated sessions
- Steal accessible browser data
- Modify page content
- Capture user input
- Perform actions as the victim
- Access browser APIs
- Conduct phishing attacks
- Interact with backend APIs using the victim's session

The impact depends on available browser permissions and application functionality.

---

# Common Indicators

Possible indicators include:

- JavaScript executes without server reflection
- Payload appears only after client-side rendering
- Browser developer tools reveal unsafe DOM updates
- Different behavior after modifying URL fragments
- Security scanner alerts
- Client-side exceptions

---

# Exploitation Requirements

Successful exploitation generally requires:

- A controllable DOM source
- An unsafe DOM sink
- Executable browser context
- Absence of effective client-side protections

The vulnerable behavior occurs entirely after the page has loaded.

---

# Mitigation

Recommended defenses include:

- Treat all browser-controlled data as untrusted
- Use safe DOM APIs
- Avoid dynamic HTML generation
- Apply context-aware output encoding
- Sanitize untrusted content
- Use secure client-side frameworks
- Implement Content Security Policy (CSP)
- Follow Trusted Types where supported

Client-side code should never insert untrusted input into executable contexts.

---

# Detection Methods

Security professionals identify DOM XSS through:

- Manual browser testing
- Browser developer tools
- JavaScript code review
- Dynamic Application Security Testing (DAST)
- DOM analysis
- Automated client-side security scanners

Testing focuses on identifying unsafe source-to-sink data flows.

---

# DOM XSS vs Reflected XSS

| DOM XSS | Reflected XSS |
|----------|---------------|
| Vulnerability exists in client-side JavaScript | Vulnerability exists in server response generation |
| Payload may never reach the server | Payload is reflected by the server |
| Executes after DOM manipulation | Executes after HTTP response rendering |
| Requires unsafe source-to-sink flow | Requires unsafe server-side reflection |

Although both execute in the browser, the vulnerable component differs significantly.

---

# Relationship to Other Vulnerabilities

DOM XSS may be chained with other client-side attacks.

```
DOM XSS

↓

Session Hijacking

↓

Account Takeover

↓

API Abuse

↓

Privilege Abuse

↓

Application Compromise
```

Modern JavaScript-heavy applications often expose multiple DOM-based attack surfaces.

---

# Real-World Examples

DOM XSS has been identified in:

- Single Page Applications (SPAs)
- Angular applications
- React applications
- Vue.js applications
- Browser extensions
- Client-side dashboards
- Enterprise portals

As applications increasingly rely on client-side rendering, DOM XSS has become one of the most common XSS variants.

---

# Importance in Offensive Security

Understanding DOM XSS enables penetration testers to:

- Assess client-side JavaScript security
- Analyze source-to-sink data flows
- Test browser-based attack surfaces
- Evaluate modern web frameworks
- Identify unsafe DOM manipulation
- Recommend secure client-side coding practices

---

> **Key Insight:** DOM XSS differs from other XSS variants because the server may never process the malicious payload. The vulnerability exists entirely in client-side JavaScript, where untrusted browser-controlled data flows into dangerous DOM operations that cause the browser to execute attacker-controlled code.