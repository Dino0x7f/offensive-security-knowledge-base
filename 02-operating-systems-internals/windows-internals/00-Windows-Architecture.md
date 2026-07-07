# Windows Architecture (Security Perspective)

## Overview

Windows is a layered operating system designed to separate user applications from privileged system components.

From a security perspective:

> Understanding Windows architecture allows a pentester to identify trust boundaries, privilege transitions, and potential attack paths.

Rather than memorizing Windows internals, security professionals focus on how each component can influence execution, authentication, privilege, or persistence.

---

# High-Level Architecture

```
+--------------------------------------+
| User Applications                    |
| (Chrome, Office, PowerShell, etc.)   |
+--------------------------------------+
| User Mode                            |
| Win32 API • DLLs • Services          |
+--------------------------------------+
| System Call Interface                |
+--------------------------------------+
| Kernel Mode                          |
| Executive • Kernel • Drivers         |
+--------------------------------------+
| Hardware Abstraction Layer (HAL)     |
+--------------------------------------+
| Physical Hardware                    |
+--------------------------------------+
```

Windows separates execution into two privilege levels:

- User Mode
- Kernel Mode

This separation is one of the most important security boundaries in the operating system.

---

# User Mode

User Mode is where normal applications execute.

Examples:

- Browsers
- Microsoft Office
- PowerShell
- Command Prompt
- Third-party applications

Characteristics:

- Limited privileges
- Cannot directly access hardware
- Cannot modify kernel memory
- Crashes usually affect only the application

Security relevance:

Most initial compromises begin in User Mode through:

- Malicious documents
- Browser exploits
- Phishing payloads
- User-executed malware

---

# Kernel Mode

Kernel Mode executes with the highest privilege level.

Responsible for:

- Process management
- Memory management
- Device drivers
- Scheduling
- Security enforcement
- Hardware communication

Characteristics:

- Full access to system memory
- Full hardware access
- Executes with SYSTEM-level privileges

Security relevance:

Kernel compromise often results in complete system compromise.

Attackers target:

- Vulnerable drivers
- Kernel exploits
- Signed driver abuse
- BYOVD (Bring Your Own Vulnerable Driver)

---

# Windows Executive

The Executive is a collection of kernel components responsible for core operating system services.

Major responsibilities include:

- Object Manager
- Process Manager
- Memory Manager
- Security Reference Monitor
- I/O Manager
- Cache Manager

Security relevance:

Many privilege escalation vulnerabilities occur inside Executive components.

---

# Hardware Abstraction Layer (HAL)

The HAL isolates Windows from hardware-specific implementations.

Instead of communicating directly with hardware, Windows communicates through the HAL.

Benefits:

- Hardware independence
- Driver compatibility
- Simplified kernel development

Security relevance:

Attackers rarely target the HAL directly, but vulnerable drivers interacting with it may become attack vectors.

---

# Device Drivers

Drivers allow Windows to communicate with hardware.

Examples:

- Network adapters
- Graphics cards
- Storage controllers
- USB devices

Drivers execute in Kernel Mode.

Security relevance:

Drivers represent one of the largest kernel attack surfaces.

Common issues:

- Vulnerable drivers
- Arbitrary read/write primitives
- Unsigned driver loading
- Driver privilege escalation

---

# Windows API

Applications rarely communicate directly with the kernel.

Instead they use the Windows API.

Example operations:

- Create a file
- Create a process
- Allocate memory
- Read registry values

Flow:

```
Application
      ↓
Windows API
      ↓
System Call
      ↓
Kernel
```

Security relevance:

Many EDR products monitor Windows API usage to detect malicious behavior.

---

# Dynamic Link Libraries (DLLs)

DLLs contain reusable Windows functionality.

Examples:

- kernel32.dll
- user32.dll
- advapi32.dll
- ntdll.dll

Security relevance:

Attackers commonly abuse DLLs through:

- DLL Hijacking
- DLL Search Order Hijacking
- DLL Side Loading
- Reflective DLL Injection

---

# System Calls

A system call is the controlled transition from User Mode to Kernel Mode.

Applications cannot directly execute privileged operations.

Instead they request services through system calls.

Examples:

- Opening files
- Creating processes
- Allocating memory

Security relevance:

Modern malware sometimes performs direct system calls to evade user-mode security monitoring.

---

# Windows Security Boundary

Windows protects resources using several privilege boundaries.

Examples:

- User → Administrator
- Administrator → SYSTEM
- User Mode → Kernel Mode
- Local Machine → Domain

Attackers constantly attempt to cross these boundaries.

---

# Why Architecture Matters in Pentesting

Understanding architecture helps answer questions like:

- Where does code execute?
- Which component owns this resource?
- Which privilege level is required?
- Can execution cross a security boundary?
- What assumptions does Windows make?

---

# Pentester Perspective

Instead of viewing Windows as applications and files, a pentester views it as layers of trust.

Typical attack progression:

1. Gain User Mode execution
2. Enumerate privileges
3. Escalate to Administrator
4. Escalate to SYSTEM
5. Dump credentials
6. Maintain persistence
7. Pivot to additional systems

Architecture explains **where** each of these actions occurs.

---

# Common Attack Targets

| Component | Common Abuse |
|-----------|--------------|
| User Mode | Malware execution |
| Windows API | Process creation, memory allocation |
| DLLs | DLL hijacking, injection |
| Drivers | Kernel privilege escalation |
| Kernel | Local privilege escalation |
| Services | Persistence, privilege escalation |
| LSASS | Credential dumping |
| Registry | Persistence |
| System Calls | EDR evasion |

---

# Key Insight

> Windows architecture is not simply a software design—it defines the trust boundaries that attackers must understand, bypass, or abuse to move from initial execution to full system compromise.
