# Password Reset Poisoning

## Overview

Password Reset Poisoning is an authentication vulnerability in which an attacker manipulates the password reset process to cause reset links, reset tokens, or password recovery emails to reference an attacker-controlled domain or destination.

Instead of attacking passwords directly, the attacker abuses flaws in how the application generates password reset URLs. If successful, password reset tokens intended for the victim may be transmitted to an attacker-controlled server, allowing complete account takeover.

Password Reset Poisoning is primarily caused by improper trust in user-controlled HTTP headers or application configuration during password reset URL generation.

---

# Password Reset Flow

Typical password reset process:

```
User Requests Password Reset

↓

Application Generates Reset Token

↓

Password Reset URL Created

↓

Reset Email Sent

↓

User Opens Link

↓

Token Verified

↓

Password Changed
```

If the generated reset URL is manipulated before delivery, the attacker may obtain the reset token.

---

# Root Cause

Password Reset Poisoning occurs because applications construct password reset URLs using untrusted or user-controlled input.

Common causes include:

- Trusting the `Host` header
- Trusting proxy headers
- Dynamic URL generation
- Weak reverse proxy configuration
- Improper domain validation
- Misconfigured load balancers
- Insecure password recovery implementation

---

# Attack Surface

Password Reset Poisoning commonly affects:

- Password recovery pages
- Account recovery workflows
- Email verification systems
- User invitation links
- Account activation links
- Enterprise authentication portals
- Single Sign-On integrations

Any feature generating absolute URLs may become vulnerable.

---

# Attack Process

Typical attack flow:

```
Attacker Manipulates Request

↓

Application Builds Reset URL

↓

Attacker-Controlled Domain Used

↓

Reset Email Sent

↓

Victim Clicks Link

↓

Reset Token Leaked

↓

Account Takeover
```

The application unknowingly embeds attacker-controlled values into trusted recovery links.

---

# Common Attack Techniques

Attackers may abuse:

- Host header manipulation
- X-Forwarded-Host
- X-Host
- Forwarded headers
- Reverse proxy misconfiguration
- Web server trust assumptions
- Email template generation

The exact technique depends on how the application determines its own hostname.

---

# Potential Impact

Successful Password Reset Poisoning may allow attackers to:

- Steal password reset tokens
- Reset user passwords
- Take over accounts
- Compromise administrator accounts
- Bypass authentication
- Escalate privileges
- Access sensitive information
- Maintain persistent access

Because password reset bypasses existing credentials, the impact is often severe.

---

# Common Indicators

Possible indicators include:

- Password reset emails containing unexpected domains
- Reset URLs pointing to external hosts
- Host header inconsistencies
- Dynamic hostname generation
- Proxy header trust
- Password reset failures after email delivery

---

# Exploitation Requirements

Successful exploitation generally requires:

- User-controlled host information
- Dynamic reset URL generation
- Victim interaction
- Weak hostname validation

Without user-controlled URL construction, Password Reset Poisoning is generally not possible.

---

# Mitigation

Recommended defenses include:

- Use a fixed application base URL
- Ignore untrusted `Host` headers
- Validate reverse proxy headers
- Configure trusted proxies correctly
- Generate reset links from server-side configuration
- Validate destination domains
- Use short-lived reset tokens
- Invalidate tokens after use
- Monitor password recovery activity

Applications should never generate security-sensitive URLs using client-controlled headers.

---

# Detection Methods

Security professionals identify Password Reset Poisoning through:

- Password recovery testing
- Host header manipulation
- Reverse proxy assessment
- Source code review
- Dynamic application testing
- Email content analysis

Testing focuses on determining whether reset URLs change when request headers are manipulated.

---

# Password Reset Poisoning vs Password Reset Token Theft

| Password Reset Poisoning | Password Reset Token Theft |
|---------------------------|----------------------------|
| Manipulates reset URL generation | Steals an existing reset token |
| Targets application logic | Targets token confidentiality |
| Uses header or hostname manipulation | Uses interception or disclosure |
| Occurs before email delivery | Occurs after token generation |

Both attacks may ultimately result in unauthorized password changes.

---

# Relationship to Other Vulnerabilities

Password Reset Poisoning often initiates complete authentication compromise.

```
Host Header Manipulation

↓

Poisoned Reset URL

↓

Reset Token Disclosure

↓

Password Reset

↓

Authenticated Access

↓

Account Compromise
```

It is frequently associated with Host Header Injection, reverse proxy misconfigurations, and weak password recovery implementations.

---

# Real-World Examples

Password Reset Poisoning vulnerabilities have affected:

- Enterprise web applications
- SaaS platforms
- Banking portals
- Cloud management systems
- E-commerce websites
- Administrative dashboards
- Custom authentication systems

Many historical vulnerabilities resulted from applications trusting client-supplied `Host` or proxy headers when generating password recovery links.

---

# Importance in Offensive Security

Understanding Password Reset Poisoning enables penetration testers to:

- Assess password recovery mechanisms
- Evaluate host header handling
- Test reverse proxy configurations
- Analyze email generation workflows
- Assess URL generation logic
- Recommend secure password recovery implementations

---

> **Key Insight:** Password Reset Poisoning does not break authentication directly it compromises the trustworthiness of the password recovery process. If attackers can influence where password reset links point, they can obtain valid reset tokens and take over accounts without ever knowing the original password.