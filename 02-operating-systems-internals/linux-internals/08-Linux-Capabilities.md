# Linux Capabilities (Security Perspective)

## Overview

Traditionally, Linux followed a simple privilege model:

- Root (UID 0) → Full privileges
- Non-root → Limited privileges

Linux **Capabilities** break root privileges into smaller, independent units. Instead of granting a process unlimited power, specific capabilities can be assigned to perform only required privileged operations.

From a security perspective:

> Linux Capabilities implement the principle of least privilege by replacing the "all-or-nothing" root model with fine-grained privileges.

Understanding capabilities is essential for privilege escalation, container security, service hardening, and Linux internals.

---

# Why Capabilities Exist

Before capabilities:

```
Root

↓

Everything Allowed
```

After capabilities:

```
Root Privileges

↓

Split into Individual Capabilities

↓

Assigned Independently
```

This significantly reduces the attack surface.

---

# High-Level Architecture

```
Application

↓

Linux Capability

↓

Kernel Permission Check

↓

Privileged Operation
```

Instead of checking:

```
Is user root?
```

The kernel checks:

```
Does this process have the required capability?
```

---

# Root vs Capabilities

| Traditional Model | Capability Model |
|-------------------|------------------|
| Root has all privileges | Privileges divided into capabilities |
| All-or-nothing | Fine-grained access |
| Higher attack surface | Least privilege |
| Harder to secure | More secure and flexible |

---

# Common Linux Capabilities

Some important capabilities include:

| Capability | Purpose |
|------------|---------|
| CAP_CHOWN | Change file ownership |
| CAP_DAC_OVERRIDE | Bypass file permissions |
| CAP_NET_ADMIN | Network administration |
| CAP_NET_RAW | Create raw sockets |
| CAP_SYS_ADMIN | General system administration |
| CAP_SYS_PTRACE | Trace other processes |
| CAP_SYS_MODULE | Load/unload kernel modules |
| CAP_SETUID | Change user ID |
| CAP_SETGID | Change group ID |
| CAP_SYS_TIME | Modify system time |
| CAP_MKNOD | Create device files |

---

# Example

Instead of giving a web server full root privileges:

```
root

↓

Everything
```

It may only receive:

```
CAP_NET_BIND_SERVICE
```

This allows binding to privileged ports (below 1024) without granting complete root access.

---

# Capability Sets

Every process maintains several capability sets.

| Set | Purpose |
|-----|---------|
| Permitted | Maximum capabilities a process may use |
| Effective | Capabilities currently active |
| Inheritable | Capabilities passed to child processes |
| Bounding | Upper limit of available capabilities |
| Ambient | Capabilities inherited across execve() |

These sets determine how privileges are used during execution.

---

# File Capabilities

Capabilities can also be assigned directly to executables.

Example concept:

```
Executable

↓

Capability Assigned

↓

Any user executing it gains that capability
```

This avoids requiring the executable to be owned by root or marked SUID.

---

# Relationship with SUID

Historically:

```
SUID Binary

↓

Runs as Root
```

Modern alternative:

```
Executable

↓

Specific Capability

↓

Limited Privilege
```

Capabilities are generally safer because they grant only the permissions that are required.

---

# Security Importance

Capabilities help:

- Reduce root privileges
- Limit service permissions
- Secure containers
- Prevent unnecessary privilege escalation
- Implement least privilege

---

# Common Security Risks

Improper capability assignment may allow:

- Privilege escalation
- Container escape
- Network manipulation
- Process injection
- Kernel interaction

Some capabilities are nearly as powerful as root.

---

# High-Risk Capabilities

| Capability | Risk |
|------------|------|
| CAP_SYS_ADMIN | Extremely powerful ("new root") |
| CAP_SYS_MODULE | Load malicious kernel modules |
| CAP_SYS_PTRACE | Read or manipulate other processes |
| CAP_DAC_OVERRIDE | Ignore filesystem permissions |
| CAP_SETUID | Become another user |
| CAP_NET_ADMIN | Modify network configuration |

---

# Capabilities in Containers

Containers typically remove many capabilities.

Example:

```
Docker Container

↓

Limited Capability Set

↓

Restricted Kernel Access
```

Removing unnecessary capabilities is a major security control for containerized workloads.

---

# Offensive Perspective

Attackers enumerate capabilities to determine whether they can:

- Escalate privileges
- Access restricted files
- Load kernel modules
- Trace privileged processes
- Manipulate networking
- Escape containers

Misconfigured capabilities are a common privilege escalation vector.

---

# Defensive Perspective

Administrators should:

- Grant only required capabilities
- Avoid CAP_SYS_ADMIN whenever possible
- Remove unnecessary container capabilities
- Audit file capabilities regularly
- Replace SUID binaries with capabilities where appropriate
- Apply the principle of least privilege

---

# Capabilities vs SUID

| SUID | Capabilities |
|------|--------------|
| Full root privileges | Specific privileges only |
| Larger attack surface | Smaller attack surface |
| Harder to secure | Easier to restrict |
| Legacy mechanism | Modern mechanism |

---

# Common Attack Scenarios

| Technique | Goal |
|-----------|------|
| Abusing CAP_SYS_ADMIN | Root-like privileges |
| CAP_SETUID abuse | Become root |
| CAP_DAC_OVERRIDE | Read protected files |
| CAP_SYS_PTRACE | Dump process memory |
| Misconfigured file capabilities | Privilege escalation |

---

# Why Capabilities Matter

Understanding Linux Capabilities helps security professionals:

- Analyze privilege escalation paths
- Secure Linux services
- Harden containers
- Replace unnecessary SUID binaries
- Audit privileged applications
- Understand modern Linux privilege management

---

# Key Takeaways

- Linux Capabilities divide root privileges into fine-grained permissions.
- The kernel checks capabilities instead of relying solely on UID 0.
- Processes maintain multiple capability sets that control privilege usage.
- File capabilities allow executables to receive specific privileges without running as root.
- Misconfigured capabilities can become powerful privilege escalation vectors.
- Capabilities are a key component of modern Linux security and container isolation.

---

# Key Insight

> Linux Capabilities transform the traditional root privilege model into a least-privilege architecture. Rather than asking "Is this process root?", the kernel asks "Does this process have exactly the capability required for this operation?" This granular approach significantly improves both system security and flexibility.
