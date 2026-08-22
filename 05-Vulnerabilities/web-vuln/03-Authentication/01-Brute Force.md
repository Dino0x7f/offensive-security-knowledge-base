# Brute Force Attack

## Overview

A Brute Force Attack is an authentication attack in which an attacker repeatedly attempts different passwords or authentication credentials until the correct combination is found. Rather than exploiting software vulnerabilities, brute force attacks exploit weak credentials and inadequate authentication protections.

Brute force remains one of the oldest and most effective attack techniques because many systems still rely on weak passwords, lack rate limiting, or fail to implement account protection mechanisms.

Although conceptually simple, modern brute force attacks are highly automated and often distributed across thousands of systems.

---

# How Brute Force Works

Typical attack flow:

```
Attacker

↓

Chooses Target Account

↓

Generates Password Attempts

↓

Authentication Requests

↓

Successful Login

↓

Account Compromise
```

The attack continues until valid credentials are discovered or defensive mechanisms interrupt the process.

---

# Root Cause

Brute force attacks succeed because authentication systems allow repeated password attempts without sufficient protection.

Common causes include:

- Weak passwords
- No account lockout
- Missing rate limiting
- Predictable passwords
- Password reuse
- Missing Multi-Factor Authentication (MFA)
- Poor monitoring
- Unlimited authentication attempts

---

# Attack Surface

Brute force attacks commonly target:

- Login pages
- VPN portals
- Remote Desktop (RDP)
- SSH servers
- Webmail
- APIs
- Administrative panels
- Cloud services
- Single Sign-On (SSO)
- Mobile applications

Any authentication endpoint may become a brute force target.

---

# Types of Brute Force Attacks

## Traditional Brute Force

The attacker systematically attempts every possible password until one succeeds.

---

## Dictionary Attack

Passwords are selected from predefined wordlists containing commonly used passwords.

---

## Hybrid Attack

Dictionary words are modified using numbers, symbols, capitalization, or common patterns.

---

## Reverse Brute Force

A common password is tested against many different usernames.

---

## Distributed Brute Force

Authentication attempts originate from multiple IP addresses to evade rate limiting and detection.

---

# Potential Impact

Successful brute force attacks may allow attackers to:

- Access user accounts
- Compromise administrator accounts
- Steal sensitive information
- Escalate privileges
- Abuse authenticated APIs
- Move laterally within an organization
- Establish persistence
- Launch additional attacks

The impact depends on the privileges associated with the compromised account.

---

# Common Indicators

Possible indicators include:

- Numerous failed login attempts
- Repeated authentication requests
- Login attempts from multiple IP addresses
- Rapid password guessing
- Authentication logs with sequential failures
- Account lockout events

---

# Mitigation

Recommended defenses include:

- Strong password policies
- Multi-Factor Authentication (MFA)
- Account lockout
- Rate limiting
- CAPTCHA after repeated failures
- Progressive authentication delays
- Password breach detection
- Login monitoring and alerting
- IP reputation filtering
- Risk-based authentication

Applications should assume password guessing attempts will occur and implement layered defenses.

---

# Detection Methods

Security professionals identify brute force vulnerabilities through:

- Authentication testing
- Rate limiting evaluation
- Account lockout testing
- Password policy assessment
- Log analysis
- Dynamic Application Security Testing (DAST)

Testing should verify that authentication mechanisms effectively resist automated password guessing.

---

# Brute Force vs Credential Stuffing

| Brute Force | Credential Stuffing |
|-------------|---------------------|
| Guesses passwords | Uses previously leaked credentials |
| Targets one or more accounts | Uses known username/password pairs |
| Success depends on password strength | Success depends on password reuse |
| Generates many failed logins | Often produces successful logins quickly |

Credential Stuffing is generally more efficient, while brute force relies on discovering unknown passwords.

---

# Relationship to Other Vulnerabilities

Brute force attacks are often part of larger attack chains.

```
Weak Authentication

↓

Brute Force Attack

↓

Successful Login

↓

Privilege Escalation

↓

Sensitive Data Access

↓

Application Compromise
```

Attackers frequently combine brute force with password spraying, credential stuffing, or session attacks.

---

# Real-World Examples

Brute force attacks frequently target:

- Banking portals
- Corporate VPN gateways
- SSH servers
- Microsoft 365 accounts
- Webmail services
- WordPress login pages
- Cloud management consoles
- Administrative dashboards

Automated tools continuously scan the Internet for exposed authentication endpoints.

---

# Importance in Offensive Security

Understanding brute force attacks enables penetration testers to:

- Assess authentication resilience
- Evaluate password policies
- Test rate limiting
- Verify account lockout mechanisms
- Assess Multi-Factor Authentication deployment
- Recommend secure authentication controls

---

> **Key Insight:** Brute force attacks do not exploit software flaws they exploit weak authentication defenses. Even perfectly written applications can be compromised if they permit unlimited password guessing, rely on weak credentials, or fail to implement layered authentication protections.