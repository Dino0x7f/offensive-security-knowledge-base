# Service Control Manager (SCM) 

## Overview

The **Service Control Manager (SCM)** is the Windows subsystem responsible for managing system services.

It is responsible for:

- Creating services
- Starting services
- Stopping services
- Monitoring service status
- Managing service configuration
- Coordinating service dependencies

From a security perspective:

> The Service Control Manager is the central authority that controls service execution. Because services often run with elevated privileges, SCM is a major target for privilege escalation, persistence, and lateral movement.

Understanding SCM is essential for Windows Internals, privilege escalation, malware analysis, incident response, and Windows administration.

---

# What is a Windows Service?

A Windows Service is a background process that:

- Starts before user login (optional)
- Runs without user interaction
- Provides operating system functionality
- Can run continuously

Examples:

- Windows Update
- Print Spooler
- Windows Defender
- DHCP Client
- Remote Registry

---

# High-Level Architecture

```
Windows Boot

      │

Service Control Manager (services.exe)

      │

────────────────────────────────

│            │             │

DNS        WinRM       Defender

│            │             │

Running Services
```

SCM controls every Windows service.

---

# SCM Process

The SCM is implemented by:

```
services.exe
```

Location:

```
C:\Windows\System32\services.exe
```

It runs as:

```
NT AUTHORITY\SYSTEM
```

---

# Boot Sequence

Simplified:

```
Windows Kernel

↓

Session Manager (smss.exe)

↓

Wininit.exe

↓

services.exe

↓

Start Windows Services
```

SCM starts very early during Windows boot.

---

# Responsibilities

SCM manages:

- Service creation
- Service startup
- Service shutdown
- Startup type
- Dependencies
- Failure recovery
- Service status
- Communication with services

---

# Service Lifecycle

```
Installed

↓

Registered

↓

Started

↓

Running

↓

Paused (optional)

↓

Stopped
```

SCM manages every stage.

---

# Startup Types

Windows supports several startup modes.

### Automatic

Starts during system boot.

---

### Automatic (Delayed Start)

Starts shortly after boot.

Used to improve boot performance.

---

### Manual

Starts only when requested.

---

### Disabled

Cannot be started until re-enabled.

---

# Service States

Possible states:

```
Stopped

Starting

Running

Pausing

Paused

Stopping
```

SCM tracks each state.

---

# Service Registration

Installed services are registered inside the Registry.

Location:

```
HKLM\SYSTEM\CurrentControlSet\Services
```

Each service has its own registry key.

---

# Service Configuration

Configuration includes:

- Service name
- Display name
- Executable path
- Startup type
- Dependencies
- Account
- Recovery options

---

# Example Structure

```
Service

├── Name

├── Binary Path

├── Startup Type

├── Dependencies

└── Account
```

---

# Service Dependencies

Some services require others.

Example:

```
Network Service

↓

RPC

↓

TCP/IP

↓

Networking Service
```

SCM ensures dependency order.

---

# Service Accounts

Services may run under different identities.

Examples:

### LocalSystem

Highest local privilege.

---

### LocalService

Limited local permissions.

---

### NetworkService

Limited local permissions with network identity.

---

### Custom User Account

Organization-defined service account.

---

# Service Execution Flow

```
SCM

↓

Load Service

↓

Create Process

↓

Initialize Service

↓

Running
```

---

# Communication with Services

SCM communicates with services through:

```
Control Codes
```

Examples:

- Start
- Stop
- Pause
- Continue
- Shutdown

---

# Service Failure Recovery

SCM supports automatic recovery.

Example:

```
Service Crash

↓

Restart Service

↓

Restart Computer

↓

Run Recovery Program
```

Recovery actions improve availability.

---

# Registry Relationship

```
Registry

↓

SYSTEM

↓

CurrentControlSet

↓

Services

↓

SCM
```

SCM reads configuration from the Registry.

---

# Relationship with Processes

```
SCM

↓

Service

↓

Process

↓

Threads
```

A service ultimately runs inside a Windows process.

---

# Relationship with LSASS

```
SCM

↓

Service Account

↓

LSASS

↓

Access Token

↓

Start Service
```

When a service starts, Windows creates an appropriate security context.

---

# Security Importance

Services often:

- Run continuously
- Execute with elevated privileges
- Listen on network ports
- Access sensitive resources
- Start automatically

This makes them attractive attack targets.

---

# Common Security Risks

Examples include:

- Weak service permissions
- Writable service binaries
- Insecure service configuration
- Unquoted service paths
- Excessive service privileges
- Outdated vulnerable services

These weaknesses can allow privilege escalation or persistence.

---

# Offensive Perspective

During post-exploitation, attackers analyze services to identify opportunities such as:

- Privileged services
- Misconfigured permissions
- Writable service executables
- Weak recovery actions
- Automatically starting services

Abusing service misconfigurations is a common path to privilege escalation.

---

# Defensive Perspective

Security teams should:

- Disable unnecessary services
- Apply least privilege to service accounts
- Secure service executables
- Restrict service modification permissions
- Patch vulnerable services
- Monitor service creation and configuration changes
- Audit privileged services

---

# Pentester Perspective

During an assessment, pentesters evaluate:

- Which services run as SYSTEM?
- Are service binaries writable?
- Are service permissions overly permissive?
- Are there unquoted service paths?
- Can services be modified by low-privileged users?
- Are unnecessary services exposed over the network?

Service enumeration is a standard privilege escalation step on Windows.

---

# Relationship with Other Windows Components

```
Windows Boot

        │

services.exe (SCM)

        │

Registry

        │

Service Configuration

        │

LSASS

        │

Access Token

        │

Service Process

        │

Running Service
```

SCM connects boot, registry configuration, authentication, and process creation.

---

# Common Service Accounts

| Account | Typical Privilege |
|----------|-------------------|
| LocalSystem | Highest local privilege |
| LocalService | Limited local privilege |
| NetworkService | Limited local privilege with network identity |
| Custom Service Account | Depends on assigned permissions |

---

# Key Takeaways

- The Service Control Manager (SCM) is implemented by **services.exe**.
- SCM is responsible for managing the lifecycle of Windows services.
- Service configuration is stored in the Registry under `HKLM\SYSTEM\CurrentControlSet\Services`.
- Services can start automatically, manually, or remain disabled.
- Services often run with elevated privileges and therefore represent a significant security boundary.
- Misconfigured services are a common source of privilege escalation and persistence.
- Proper service hardening, least privilege, and continuous monitoring are essential for securing Windows systems.

---

# Key Insight

> The Service Control Manager is more than a service launcher it is the orchestration layer that governs how privileged background processes are created, configured, and executed. Every Windows service ultimately operates under the trust decisions enforced by the SCM.
