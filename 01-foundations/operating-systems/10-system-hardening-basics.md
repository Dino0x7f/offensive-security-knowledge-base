# System Hardening Basics (Security Perspective)

## Introduction

System hardening is the process of securing an operating system by reducing its attack surface, eliminating unnecessary functionality, and enforcing secure configurations.

The objective is to minimize the number of opportunities an attacker can exploit while maintaining the system's intended functionality.

From a security perspective:

> System hardening is the practice of reducing opportunities for compromise before an attack occurs.

---

# Why System Hardening Matters

A default operating system installation often includes:

- Unnecessary services
- Default configurations
- Excessive permissions
- Legacy protocols
- Unused software

These components increase the attack surface and provide additional opportunities for attackers.

Effective hardening significantly improves the security posture of a system.

---

# Core Hardening Principles

System hardening follows several fundamental principles:

- Reduce the attack surface
- Apply the Principle of Least Privilege
- Disable unnecessary functionality
- Secure default configurations
- Keep systems updated
- Continuously monitor and review security settings

---

# Key Hardening Areas

## User and Account Management

Security recommendations include:

- Remove unused accounts
- Disable default accounts where possible
- Enforce strong password policies
- Implement Multi-Factor Authentication (MFA)
- Apply the Principle of Least Privilege

### Security Benefit

Reducing unnecessary user privileges limits the impact of compromised accounts.

---

## Service Hardening

Recommendations include:

- Disable unused services
- Remove unnecessary software
- Restrict service permissions
- Minimize exposed network services

### Security Benefit

Every disabled service removes a potential attack vector.

---

## Network Hardening

Important controls include:

- Enable host-based firewalls
- Restrict inbound traffic
- Limit outbound communication
- Secure remote administration (SSH, RDP)
- Disable insecure protocols

### Security Benefit

Network hardening reduces the system's external attack surface.

---

## File System Hardening

Recommendations include:

- Apply proper file permissions
- Remove world-writable files
- Protect sensitive directories
- Encrypt sensitive data where appropriate

### Security Benefit

Restricting file access limits unauthorized disclosure and privilege escalation.

---

## Authentication Hardening

Best practices include:

- Strong password policies
- Account lockout policies
- Multi-Factor Authentication
- Secure credential storage
- Password rotation policies where appropriate

### Security Benefit

Stronger authentication significantly increases the difficulty of unauthorized access.

---

## Patch Management

Keeping software up to date is one of the most effective hardening measures.

Updates should include:

- Operating system patches
- Security updates
- Driver updates
- Third-party applications

### Security Benefit

Many real-world attacks exploit vulnerabilities that already have available patches.

---

## Logging and Auditing

Effective hardening includes:

- Enable security auditing
- Collect authentication logs
- Monitor privileged activity
- Protect log integrity

### Security Benefit

Proper logging improves detection and incident response capabilities.

---

## Application Hardening

Applications should be configured securely by:

- Removing unused features
- Disabling default credentials
- Restricting administrative interfaces
- Applying vendor security recommendations

### Security Benefit

Applications often represent the largest portion of a system's attack surface.

---

# Common Security Misconfigurations

Frequently observed weaknesses include:

- Default credentials
- Excessive administrative privileges
- Weak password policies
- Unpatched software
- Unnecessary services
- Open network ports
- Misconfigured firewall rules
- Insecure remote access
- Legacy protocols
- Weak file permissions

Most successful compromises begin with one or more configuration mistakes.

---

# Attacker Perspective

Attackers look for systems that are poorly hardened because they often expose:

- Unpatched vulnerabilities
- Weak authentication
- Misconfigured permissions
- Vulnerable services
- Default configurations
- Forgotten software
- Legacy protocols

Poor hardening reduces the effort required to compromise a system.

---

# Defensive Perspective

System administrators and security teams harden systems to:

- Reduce attack surface
- Prevent privilege escalation
- Limit lateral movement
- Protect sensitive data
- Improve resilience against attacks
- Increase visibility through monitoring

Hardening complements—but does not replace—continuous monitoring and incident response.

---

# Key Takeaways

- System hardening is the process of reducing attack surface through secure configuration.
- Secure systems rely on least privilege, minimal services, proper authentication, and regular updates.
- Most successful attacks exploit configuration weaknesses rather than operating system design flaws.
- Hardening should be performed before deployment and maintained throughout the system's lifecycle.
- Effective hardening forces attackers to chain multiple weaknesses instead of exploiting a single misconfiguration.

---

# Key Insight

> System hardening does not make a system impossible to compromise—it raises the cost, complexity, and time required for a successful attack.
