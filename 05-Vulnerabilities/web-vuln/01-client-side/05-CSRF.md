# Cross-Site Request Forgery (CSRF)

## Overview

Cross-Site Request Forgery (CSRF) is a client-side web vulnerability that tricks an authenticated user's browser into sending unintended requests to a trusted web application without the user's knowledge or consent.

Because browsers automatically include authentication credentials such as cookies, session identifiers, and client certificates with requests, an attacker can abuse the victim's authenticated session to perform unauthorized actions.

Unlike Cross-Site Scripting (XSS), CSRF does **not** require the attacker to execute JavaScript within the target application. Instead, it exploits the browser's trust in authenticated sessions.

---

# How CSRF Works

Typical attack flow:

```
Victim Logs In

↓

Authenticated Session Created

↓

Victim Visits Attacker-Controlled Website

↓

Malicious Request Sent

↓

Browser Automatically Includes Session Cookie

↓

Target Application Executes Request
```

The server processes the request as if it originated from the legitimate user.

---

# Root Cause

CSRF occurs because applications trust authenticated requests without verifying whether the request was intentionally initiated by the legitimate user.

Common causes include:

- Missing CSRF tokens
- Reliance solely on session cookies
- Missing Origin validation
- Missing Referer validation
- Unsafe state-changing GET requests
- Weak SameSite cookie configuration

---

# Browser Behavior

Browsers automatically attach authentication information to requests.

Examples include:

- Session cookies
- Persistent cookies
- Client TLS certificates
- HTTP Authentication credentials

This automatic behavior is the fundamental prerequisite for CSRF.

---

# Attack Surface

CSRF commonly targets functions that modify application state.

Examples include:

- Password changes
- Email changes
- Money transfers
- User management
- Administrative actions
- Account deletion
- Profile updates
- API requests
- Shopping cart operations

Read-only requests generally present lower risk than state-changing operations.

---

# Types of CSRF

## State-Changing CSRF

The attacker performs unauthorized actions.

Examples include:

- Changing account settings
- Updating passwords
- Purchasing products
- Transferring funds

---

## Login CSRF

The victim is unknowingly authenticated into an attacker-controlled account.

---

## Stored CSRF

The malicious request is embedded within stored application content.

---

## Client-Side CSRF

Client-side JavaScript initiates unintended authenticated requests.

---

# Exploitation Requirements

Successful CSRF exploitation generally requires:

- Victim is authenticated
- Browser automatically sends credentials
- State-changing endpoint exists
- No effective CSRF protection
- Victim interacts with attacker-controlled content

Without an authenticated session, traditional CSRF attacks usually fail.

---

# Potential Impact

Successful exploitation may allow attackers to:

- Modify account settings
- Change passwords
- Perform financial transactions
- Create or delete resources
- Trigger administrative actions
- Modify application data
- Abuse authenticated APIs
- Compromise user accounts

The impact depends on the permissions of the targeted user.

---

# Common Indicators

Possible indicators include:

- Sensitive operations without CSRF protection
- Missing anti-CSRF tokens
- State-changing GET requests
- Cookies lacking appropriate SameSite attributes
- Missing Origin validation
- Missing Referer validation

---

# Mitigation

Recommended defenses include:

- Anti-CSRF tokens
- SameSite cookies
- Validate Origin headers
- Validate Referer headers
- Require POST, PUT, PATCH, or DELETE for state-changing operations
- Re-authenticate for sensitive actions
- Double Submit Cookie pattern
- User confirmation for critical operations

Applications should never rely solely on session cookies for request authenticity.

---

# Detection Methods

Security professionals identify CSRF through:

- Manual testing
- Authentication workflow analysis
- Source code review
- Dynamic Application Security Testing (DAST)
- API testing
- Automated vulnerability scanners

Testing focuses on whether authenticated requests require proof of user intent.

---

# CSRF vs XSS

| CSRF | XSS |
|------|-----|
| Exploits browser trust in authenticated sessions | Executes attacker-controlled JavaScript |
| Requires authenticated victim | May not require authentication |
| Does not require script execution | Requires script execution |
| Performs unintended actions | Executes arbitrary client-side code |

Although different vulnerabilities, XSS can often bypass CSRF protections by stealing tokens or issuing authenticated requests directly.

---

# Relationship to Other Vulnerabilities

CSRF is frequently combined with other attacks.

```
Authenticated User

↓

Visits Malicious Website

↓

Forged Request

↓

Unauthorized Action

↓

Privilege Abuse

↓

Application Compromise
```

If combined with XSS, the impact increases significantly because JavaScript can automate authenticated actions.

---

# Real-World Examples

CSRF vulnerabilities have historically affected:

- Online Banking Platforms
- Social Media Applications
- E-commerce Websites
- Administrative Portals
- Cloud Management Platforms
- Enterprise Applications
- REST APIs
- Content Management Systems

The widespread adoption of SameSite cookies has reduced—but not eliminated—the prevalence of CSRF.

---

# Importance in Offensive Security

Understanding CSRF enables penetration testers to:

- Assess session management
- Evaluate authentication workflows
- Test anti-CSRF mechanisms
- Analyze browser trust boundaries
- Assess state-changing endpoints
- Recommend secure request validation practices

---

> **Key Insight:** CSRF succeeds because the browser automatically authenticates requests on behalf of the user. The application verifies *who* sent the request but fails to verify *whether the user intentionally initiated it*. Effective defenses therefore focus on validating request authenticity, not just user identity.