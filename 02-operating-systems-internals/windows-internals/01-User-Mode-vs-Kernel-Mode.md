# User Mode vs Kernel Mode (Security Perspective)

## Overview

Windows separates execution into two distinct privilege levels:

- **User Mode**
- **Kernel Mode**

From a security perspective:

> The boundary between User Mode and Kernel Mode is one of the most important security boundaries in Windows. Most attacks attempt to cross it.

---

# Why This Separation Exists

Modern operating systems isolate applications from critical system components.

This isolation provides:

- Stability
- Security
- Fault isolation
- Access control

Without this separation, any application could directly modify memory, hardware, or the operating system itself.

---

# User Mode

## What is User Mode?

User Mode is the execution environment for normal applications.

Examples:

- Chrome
- Microsoft Word
- PowerShell
- cmd.exe
- Visual Studio
- Third-party software

Applications running in User Mode have **limited privileges**.

---

## Capabilities

Applications can:

- Read and write their own memory
- Access files they have permission to use
- Communicate over the network
- Call Windows APIs

Applications cannot:

- Access kernel memory
- Execute privileged CPU instructions
- Directly communicate with hardware
- Modify operating system internals

---

## Security Characteristics

Advantages:

- Application crashes rarely affect the whole system
- Malware is initially restricted
- Strong isolation between applications

Limitations:

- Cannot bypass Windows security boundaries
- Cannot directly obtain SYSTEM privileges

---

## Common Attack Scenarios

Attackers often gain initial execution through:

- Phishing
- Malicious Office documents
- Browser exploits
- Email attachments
- User-downloaded malware

At this stage, malware usually runs in User Mode.

---

# Kernel Mode

## What is Kernel Mode?

Kernel Mode is the privileged execution environment of Windows.

Everything running in Kernel Mode has unrestricted access to the system.

---

## Responsibilities

Kernel Mode manages:

- Processes
- Threads
- Memory
- Hardware
- Drivers
- Scheduling
- Security enforcement

---

## Capabilities

Kernel Mode can:

- Access all physical memory
- Read every process
- Load drivers
- Modify kernel structures
- Communicate directly with hardware

There are virtually no permission restrictions inside Kernel Mode.

---

## Security Characteristics

Advantages:

- Maximum performance
- Complete hardware control
- Centralized operating system management

Disadvantages:

- Any bug may crash Windows
- Any exploit may compromise the entire system

---

# User Mode vs Kernel Mode

| User Mode | Kernel Mode |
|------------|-------------|
| Limited privileges | Full privileges |
| Applications | Windows kernel & drivers |
| Cannot access hardware directly | Direct hardware access |
| Cannot access kernel memory | Full memory access |
| Safe crashes | Can crash entire OS |
| Initial malware execution | Full system compromise |

---

# Communication Between Them

Applications cannot directly execute privileged operations.

Instead, they perform **system calls**.

Flow:

```
Application
      │
      ▼
Windows API
      │
      ▼
System Call
      │
      ▼
Kernel Mode
```

The kernel validates every request before performing privileged operations.

---

# Why Attackers Care

Most malware begins in User Mode.

Typical attack progression:

```
User Mode
      │
      ▼
Privilege Escalation
      │
      ▼
Administrator
      │
      ▼
Kernel Exploit
      │
      ▼
SYSTEM / Kernel Control
```

Crossing this boundary dramatically increases attacker capabilities.

---

# Common Kernel Attack Targets

Attackers commonly abuse:

- Vulnerable drivers
- Kernel vulnerabilities
- Signed driver abuse (BYOVD)
- Direct kernel object manipulation
- Driver misconfigurations

Successful exploitation often grants:

- SYSTEM privileges
- Credential dumping
- Security software bypass
- Full persistence

---

# Why Security Products Care

EDR and antivirus solutions monitor User Mode extensively.

Modern attackers therefore attempt to:

- Execute direct system calls
- Abuse legitimate drivers
- Disable security software from Kernel Mode
- Bypass User Mode monitoring

This is why kernel-level access is highly valuable.

---

# Pentester Perspective

A pentester asks:

- Where is my code executing?
- What privilege level do I have?
- Can I cross into Kernel Mode?
- Is a vulnerable driver installed?
- Is privilege escalation possible?

Understanding execution level determines what techniques are possible.

---

# Common Examples

### User Mode

- Browser exploit
- PowerShell execution
- Office macro
- Command Prompt
- Meterpreter session

---

### Kernel Mode

- Windows Kernel
- Device drivers
- File system drivers
- Network drivers
- Hypervisor interaction
- Security Reference Monitor

---

# Attack Flow Example

```
Phishing Email
      │
      ▼
User opens attachment
      │
      ▼
Malware executes (User Mode)
      │
      ▼
Enumerate system
      │
      ▼
Exploit vulnerable driver
      │
      ▼
Kernel Mode execution
      │
      ▼
SYSTEM privileges
      │
      ▼
Credential dumping
      │
      ▼
Persistence
```

---

# Key Takeaways

- User Mode is where most attacks begin.
- Kernel Mode is where complete system compromise occurs.
- The User Mode → Kernel Mode boundary is a primary security boundary in Windows.
- Most privilege escalation attacks aim to cross this boundary.
- Modern EDR solutions heavily protect User Mode, making Kernel Mode an increasingly attractive target.

---

# Key Insight

> User Mode limits what attackers can do. Kernel Mode removes almost every restriction. Most Windows privilege escalation techniques ultimately aim to cross this boundary and obtain unrestricted control of the operating system.
