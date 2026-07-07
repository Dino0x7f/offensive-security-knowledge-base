# Linux Security Modules (LSM) (Security Perspective)

## Overview

Linux Security Modules (LSM) is a security framework built into the Linux kernel that enables security policies to be enforced beyond traditional Unix permissions.

Instead of relying only on user IDs, group IDs, and file permissions, LSM allows additional access control mechanisms to restrict what processes can do.

From a security perspective:

> Linux Security Modules provide mandatory security enforcement at the kernel level, reducing the impact of compromised applications and limiting privilege escalation.

Understanding LSM is essential for Linux hardening, container security, malware analysis, and enterprise security.

---

# What are Linux Security Modules?

LSM is a kernel framework that allows security modules to intercept sensitive operations before the kernel executes them.

Instead of:

```
Process

↓

Kernel

↓

Access Granted
```

The flow becomes:

```
Process

↓

Linux Security Module

↓

Kernel Decision

↓

Allow / Deny
```

---

# Why LSM Exists

Traditional Linux security relies on:

- User IDs
- Group IDs
- File permissions

These are called:

```
Discretionary Access Control (DAC)
```

DAC has limitations because privileged users (especially root) can bypass many restrictions.

LSM introduces:

```
Mandatory Access Control (MAC)
```

where security policies are enforced regardless of user permissions.

---

# High-Level Architecture

```
Application

↓

System Call

↓

Linux Security Module

↓

Kernel

↓

Hardware / Filesystem
```

Every sensitive kernel operation can be checked by an LSM before execution.

---

# Security Models

## DAC (Discretionary Access Control)

Controlled by:

- Owner
- Group
- Permission bits

Example:

```
chmod

chown
```

The owner decides access.

---

## MAC (Mandatory Access Control)

Controlled by:

- Kernel policy
- Security labels
- LSM

Even root may be denied access.

---

# Common Linux Security Modules

Several LSM implementations exist.

| Module | Purpose |
|----------|---------|
| SELinux | Mandatory Access Control |
| AppArmor | Profile-based access control |
| Smack | Simplicity-focused MAC |
| TOMOYO | Path-based restrictions |
| Yama | Process security restrictions |
| Landlock | Unprivileged sandboxing |
| BPF LSM | eBPF-based security policies |

---

# SELinux

Security-Enhanced Linux.

Uses:

- Labels
- Contexts
- Policies

Example:

```
User

↓

File Context

↓

SELinux Policy

↓

Allow / Deny
```

Widely used in:

- Red Hat
- Fedora
- CentOS
- RHEL

---

# AppArmor

Uses profiles instead of labels.

Each application has an associated profile.

Example:

```
Firefox

↓

Allowed Files

↓

Allowed Network Access

↓

Denied Everything Else
```

Widely used in:

- Ubuntu
- Debian

---

# Yama

Focuses on:

- ptrace restrictions
- Process inspection
- Debugging controls

Security relevance:

Reduces process injection and credential theft.

---

# Landlock

A newer LSM designed for application sandboxing.

Allows even unprivileged processes to voluntarily restrict themselves.

Useful for:

- Sandboxed applications
- User-space security

---

# Security Labels

Some LSMs attach labels to objects.

Example:

```
Process

↓

Security Context

↓

File

↓

Security Context

↓

Policy Check
```

Access depends on both labels and policy—not just file permissions.

---

# LSM Hooks

The Linux kernel contains security hooks throughout its code.

Examples:

- File access
- Process creation
- Network connections
- Memory mapping
- Module loading
- Capability checks

Whenever one of these operations occurs, the LSM can approve or deny it.

---

# Relationship with the Kernel

```
Application

↓

System Call

↓

Kernel Hook

↓

Linux Security Module

↓

Decision

↓

Kernel Execution
```

LSM acts as an additional security layer rather than replacing the kernel.

---

# Security Importance

LSM provides:

- Mandatory Access Control
- Process isolation
- Application confinement
- Least privilege enforcement
- Kernel-level policy enforcement

Even privileged applications can be restricted.

---

# Common Security Risks

Improper LSM configuration may lead to:

- Overly permissive policies
- Disabled enforcement
- Policy bypasses
- Reduced application functionality

Security policies should be carefully tested and maintained.

---

# Offensive Perspective

Attackers attempt to determine:

- Which LSM is enabled
- Whether enforcement is active
- Weak or permissive policies
- Opportunities to abuse allowed operations

Common objectives include:

- Bypassing security policies
- Escalating privileges
- Accessing restricted resources

---

# Defensive Perspective

Administrators should:

- Enable an appropriate LSM (SELinux, AppArmor, etc.)
- Keep policies up to date
- Audit policy violations
- Apply least privilege
- Restrict high-risk services
- Monitor denied operations

LSM is most effective when combined with other kernel security mechanisms.

---

# LSM vs Traditional Permissions

| Traditional Permissions | Linux Security Modules |
|--------------------------|------------------------|
| DAC | MAC |
| User-controlled | Kernel-enforced |
| Based on ownership | Based on security policy |
| Root bypasses most checks | Even root can be restricted |
| Simpler | More granular |

---

# Common Attack Scenarios

| Technique | Goal |
|-----------|------|
| Disable SELinux/AppArmor | Remove restrictions |
| Abuse permissive policies | Access protected resources |
| Bypass process confinement | Privilege escalation |
| Exploit weak application profiles | Escape restrictions |

---

# LSM in Modern Linux

Modern Linux security often combines:

```
Namespaces

+

cgroups

+

Capabilities

+

Linux Security Modules

+

seccomp
```

Together, these technologies provide layered defense.

---

# Why Linux Security Modules Matter

Understanding LSM helps security professionals:

- Harden Linux systems
- Analyze privilege boundaries
- Investigate security policy violations
- Secure containers
- Restrict application behavior
- Implement defense in depth

---

# Key Takeaways

- Linux Security Modules provide a framework for kernel-level security enforcement.
- LSM enables Mandatory Access Control (MAC), extending beyond traditional Unix permissions.
- Popular implementations include SELinux, AppArmor, Yama, and Landlock.
- LSM intercepts sensitive kernel operations through security hooks.
- Security policies can restrict even privileged processes.
- LSM is a critical component of modern Linux security architecture.

---

# Key Insight

> Linux Security Modules shift security from **"Who owns the resource?"** to **"Should this action ever be allowed?"** By enforcing mandatory policies inside the kernel, LSM adds a powerful layer of defense that remains effective even when traditional permissions or privileged accounts are compromised.
