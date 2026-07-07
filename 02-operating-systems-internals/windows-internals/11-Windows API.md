# Windows API (Windows Internals - Security Perspective)

## Overview

The **Windows API (Application Programming Interface)** is a collection of functions provided by Microsoft that allows applications to interact with the Windows operating system.

Instead of communicating directly with the kernel, applications call Windows API functions to perform tasks such as:

- Creating files
- Allocating memory
- Creating processes
- Managing threads
- Accessing the Registry
- Communicating over the network

From a security perspective:

> Almost every Windows attack from malware execution to process injection—relies on Windows API calls. Understanding the Windows API means understanding how software interacts with the operating system.

---

# Why Windows API Exists

Applications should not directly manipulate kernel structures.

Instead, Windows provides standardized APIs that:

- Hide kernel complexity
- Enforce security checks
- Improve compatibility
- Simplify software development

---

# Windows API Architecture

```
Application

      │

Windows API

      │

NTDLL

      │

System Calls

      │

Windows Kernel

      │

Hardware
```

Applications rarely communicate directly with the kernel.

Instead, they use Windows API functions.

---

# Major Windows API Libraries

Some of the most important DLLs include:

| DLL | Purpose |
|------|---------|
| kernel32.dll | Processes, files, memory |
| user32.dll | GUI, keyboard, mouse |
| advapi32.dll | Registry, services, security |
| gdi32.dll | Graphics |
| ws2_32.dll | Networking (Winsock) |
| ntdll.dll | Native API & system calls |

---

# kernel32.dll

One of the most commonly used Windows libraries.

Provides functions for:

- Process creation
- File operations
- Memory allocation
- Thread management
- Synchronization

Examples:

```
CreateProcess()

CreateFile()

ReadFile()

WriteFile()

VirtualAlloc()
```

---

## Security Relevance

Most malware interacts heavily with `kernel32.dll`.

Many EDR solutions monitor calls to its functions.

---

# user32.dll

Handles graphical user interface operations.

Examples:

- Windows
- Menus
- Keyboard input
- Mouse input
- Message boxes

Common functions:

```
MessageBox()

GetMessage()

DispatchMessage()
```

---

## Security Relevance

Malware may use `user32.dll` to:

- Capture keyboard input
- Simulate user actions
- Create fake login windows

---

# advapi32.dll

Provides advanced system services.

Examples:

- Registry access
- Security tokens
- Service management
- Event logs

Common APIs:

```
RegOpenKey()

OpenSCManager()

OpenProcessToken()
```

---

## Security Relevance

Attackers frequently use this library for:

- Privilege escalation
- Registry persistence
- Token manipulation

---

# ws2_32.dll

Implements the Windows networking API (Winsock).

Provides:

- TCP
- UDP
- Socket communication

Common functions:

```
socket()

connect()

recv()

send()
```

---

## Security Relevance

Reverse shells and malware communications rely heavily on Winsock APIs.

---

# ntdll.dll

The lowest user-mode Windows library.

Contains:

- Native API
- System Call stubs

Example:

```
kernel32.dll

↓

ntdll.dll

↓

Kernel
```

---

## Security Relevance

Advanced malware sometimes bypasses high-level APIs and directly calls Native APIs.

---

# Common Categories of Windows APIs

---

## File Management

Examples:

```
CreateFile()

ReadFile()

WriteFile()

DeleteFile()

CopyFile()
```

---

## Security Relevance

Attackers use these APIs to:

- Read configuration files
- Drop malware
- Encrypt files (ransomware)

---

# Process Management

Examples:

```
CreateProcess()

OpenProcess()

TerminateProcess()

ExitProcess()
```

---

## Security Relevance

Used during:

- Process Injection
- Process Hollowing
- Malware execution

---

# Memory Management

Examples:

```
VirtualAlloc()

VirtualProtect()

VirtualFree()

WriteProcessMemory()
```

---

## Security Relevance

Critical APIs for:

- Shellcode execution
- Reflective loading
- Memory injection

---

# Thread Management

Examples:

```
CreateThread()

CreateRemoteThread()

SuspendThread()

ResumeThread()
```

---

## Security Relevance

Commonly abused for:

- Remote Thread Injection
- Thread Hijacking
- APC Injection

---

# Registry APIs

Examples:

```
RegOpenKey()

RegCreateKey()

RegSetValue()

RegDeleteKey()
```

---

## Security Relevance

Registry APIs enable:

- Persistence
- Configuration changes
- Credential storage access

---

# Service APIs

Examples:

```
OpenSCManager()

CreateService()

StartService()

DeleteService()
```

---

## Security Relevance

Attackers create malicious services for persistence.

---

# Network APIs

Examples:

```
socket()

connect()

bind()

listen()

recv()

send()
```

---

## Security Relevance

Used for:

- Reverse shells
- C2 communication
- Data exfiltration

---

# Security APIs

Examples:

```
OpenProcessToken()

AdjustTokenPrivileges()

DuplicateToken()

ImpersonateLoggedOnUser()
```

---

## Security Relevance

Used for:

- Token theft
- Privilege escalation
- User impersonation

---

# Windows API Call Flow

Example:

```
Application

↓

CreateFile()

↓

kernel32.dll

↓

ntdll.dll

↓

System Call

↓

Kernel

↓

NTFS
```

Applications never interact directly with NTFS.

---

# System Calls

Eventually every important Windows API reaches a system call.

Example:

```
VirtualAlloc()

↓

NtAllocateVirtualMemory()

↓

Kernel
```

System calls transition execution from User Mode to Kernel Mode.

---

# Common Offensive APIs

Some APIs appear frequently in malware and red-team tools.

| API | Common Use |
|------|------------|
| OpenProcess() | Access another process |
| VirtualAllocEx() | Allocate remote memory |
| WriteProcessMemory() | Write shellcode |
| CreateRemoteThread() | Execute injected code |
| VirtualProtect() | Change memory permissions |
| LoadLibrary() | Load DLL |
| GetProcAddress() | Resolve function addresses |
| OpenProcessToken() | Token access |
| DuplicateToken() | Privilege escalation |
| CreateService() | Persistence |

---

# Dynamic API Resolution

Instead of importing APIs normally, malware may resolve them dynamically.

Example:

```
LoadLibrary()

↓

GetProcAddress()

↓

Function Pointer

↓

Execute
```

---

## Advantages

- Smaller Import Table
- Evasion
- Harder static analysis

---

# API Hooking

Security products often intercept API calls.

Example:

```
Application

↓

CreateFile()

↓

Hook

↓

Security Product

↓

Original API
```

---

## Security Relevance

Malware attempts to bypass API hooks to evade detection.

---

# Direct System Calls

Modern malware may avoid Windows APIs completely.

Instead:

```
Application

↓

Direct Syscall

↓

Kernel
```

Advantages:

- Avoid API hooks
- Bypass some EDR monitoring
- Faster execution

---

# Defensive Perspective

Modern security products monitor:

- Suspicious API sequences
- Memory allocation APIs
- Remote thread creation
- Token manipulation
- DLL loading
- Registry modifications
- Service creation

Behavior-based detection relies heavily on Windows API activity.

---

# Pentester Perspective

During assessments, pentesters analyze:

- Which APIs applications call
- Imported functions
- Dynamically resolved APIs
- Suspicious API sequences
- Opportunities for API abuse

Understanding Windows APIs helps explain how exploitation techniques are implemented.

---

# Relationship with Other Components

```
Application

      │

Windows API

      │

NTDLL

      │

System Calls

      │

Windows Kernel

      │

Hardware
```

The Windows API acts as the primary interface between applications and the operating system.

---

# Common Attack Techniques Using Windows API

| Technique | APIs Involved |
|-----------|---------------|
| Process Injection | OpenProcess(), WriteProcessMemory(), CreateRemoteThread() |
| DLL Injection | LoadLibrary(), GetProcAddress() |
| Process Hollowing | CreateProcess(), VirtualAllocEx(), WriteProcessMemory() |
| Token Theft | OpenProcessToken(), DuplicateToken() |
| Registry Persistence | RegSetValue() |
| Service Persistence | CreateService() |
| Reverse Shell | socket(), connect(), recv(), send() |

---

# Key Takeaways

- The Windows API is the standard interface for interacting with Windows.
- Most Windows applications rely on API calls instead of directly communicating with the kernel.
- Core libraries include `kernel32.dll`, `advapi32.dll`, `user32.dll`, `ws2_32.dll`, and `ntdll.dll`.
- Offensive tools and malware heavily rely on Windows APIs for memory manipulation, process injection, networking, and persistence.
- Modern security products monitor suspicious API usage to detect malicious behavior.
- Understanding Windows APIs is essential for reverse engineering, exploit development, malware analysis, and red teaming.

---

# Key Insight

> The Windows API is the language through which applications communicate with the operating system. Every file opened, process created, memory allocation performed, or network connection established ultimately passes through these APIs, making them one of the most important layers to understand in both offensive and defensive Windows security.
