# Clickjacking

## Overview

Clickjacking, also known as **UI Redressing**, is a client-side vulnerability that tricks users into clicking on hidden or disguised elements from another website. By manipulating the user interface, attackers cause victims to unknowingly perform actions on a legitimate application while believing they are interacting with harmless content.

Unlike Cross-Site Scripting (XSS), Clickjacking does not execute malicious JavaScript within the target application. Instead, it abuses the browser's rendering behavior and the user's trust in the visible interface.

Because authenticated users often remain logged into web applications, Clickjacking can force them to perform sensitive actions without realizing it.

---

# How Clickjacking Works

Typical attack flow:

```
Victim Visits Malicious Website

↓

Attacker Loads Target Website Inside an Invisible Frame

↓

Fake Interface Displayed

↓

Victim Clicks Visible Element

↓

Hidden Target Receives Click

↓

Sensitive Action Executed
```

The victim believes they are interacting with one page, while the browser actually sends clicks to another.

---

# Root Cause

Clickjacking occurs because applications allow their pages to be embedded inside frames or iframes without appropriate restrictions.

Common causes include:

- Missing `X-Frame-Options`
- Missing `Content-Security-Policy` frame restrictions
- Allowing unrestricted iframe embedding
- Weak browser UI protections
- Sensitive actions without additional verification

---

# Browser Behavior

Browsers permit one website to embed another using elements such as:

- `<iframe>`
- `<frame>`
- `<object>`
- `<embed>`

If the target application does not explicitly prohibit framing, attackers may overlay or hide the embedded page.

---

# Attack Surface

Clickjacking commonly targets pages containing sensitive user actions.

Examples include:

- Login pages
- Password changes
- Account settings
- Payment confirmation
- Money transfers
- Administrative dashboards
- User management
- API consoles
- Cloud management portals

---

# Types of Clickjacking

## Transparent Overlay

The target page is rendered invisible beneath attacker-controlled content.

---

## Invisible Iframe

A hidden iframe captures user clicks.

---

## Cursor Manipulation

Visual elements are positioned so that the user's cursor activates hidden controls.

---

## Multi-Step Clickjacking

Several sequential clicks trigger multiple sensitive operations.

---

## Likejacking

Victims unknowingly interact with social media buttons.

---

# Exploitation Requirements

Successful Clickjacking generally requires:

- Target page allows framing
- Victim is authenticated
- User interaction occurs
- Sensitive action is immediately executable
- No secondary confirmation mechanism exists

---

# Potential Impact

Successful exploitation may allow attackers to:

- Modify account settings
- Change passwords
- Approve financial transactions
- Authorize applications
- Enable administrative features
- Perform account actions
- Abuse authenticated sessions
- Manipulate user preferences

The impact depends on the functionality exposed through the framed page.

---

# Common Indicators

Possible indicators include:

- Missing `X-Frame-Options`
- Missing `frame-ancestors` directive
- Sensitive pages render inside iframes
- No confirmation before critical actions
- Hidden interactive elements
- Unexpected iframe behavior

---

# Mitigation

Recommended defenses include:

- `Content-Security-Policy: frame-ancestors`
- `X-Frame-Options`
- User confirmation for sensitive actions
- Re-authentication before critical operations
- Multi-factor authentication for high-risk actions
- SameSite cookies
- Frame-busting techniques (legacy support)

Modern applications should primarily rely on **Content Security Policy (CSP)** rather than legacy frame-busting scripts.

---

# Detection Methods

Security professionals identify Clickjacking through:

- Manual iframe testing
- Browser developer tools
- Security header analysis
- Dynamic application testing
- Automated vulnerability scanners

Testing focuses on whether sensitive pages can be embedded by external origins.

---

# Clickjacking vs CSRF

| Clickjacking | CSRF |
|---------------|------|
| Requires user interaction | May require only automatic browser requests |
| Manipulates the user interface | Manipulates authenticated requests |
| Relies on visual deception | Relies on browser credential handling |
| Uses hidden frames | Uses forged requests |

Although different techniques, both exploit the browser's trust in authenticated users.

---

# Relationship to Other Vulnerabilities

Clickjacking is often combined with additional attacks.

```
Victim Visits Malicious Page

↓

Hidden Target Frame

↓

Victim Clicks

↓

Sensitive Action Executed

↓

Privilege Abuse

↓

Account Compromise
```

In some attack chains, Clickjacking is combined with XSS or CSRF to increase reliability or bypass user interaction requirements.

---

# Real-World Examples

Clickjacking vulnerabilities have historically affected:

- Banking portals
- Social media platforms
- Administrative dashboards
- Cloud management consoles
- Enterprise applications
- Email services
- Content Management Systems

Modern browsers and security headers have significantly reduced exposure, but improperly configured applications remain vulnerable.

---

# Importance in Offensive Security

Understanding Clickjacking enables penetration testers to:

- Evaluate browser security headers
- Assess UI security
- Test framing restrictions
- Analyze user interaction risks
- Assess sensitive workflows
- Recommend secure browser embedding policies

---

> **Key Insight:** Clickjacking exploits user trust rather than application logic. By disguising or hiding legitimate web pages inside attacker-controlled interfaces, attackers can manipulate authenticated users into performing unintended actions without executing any code on the target application.