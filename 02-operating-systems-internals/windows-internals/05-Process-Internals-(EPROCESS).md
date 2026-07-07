# Process Internals (EPROCESS) (Security Perspective)

## Overview

Every running process in Windows is represented internally by a kernel object called **EPROCESS**.

From a security perspective:

> EPROCESS is the kernel's representation of a process. It contains everything Windows needs to identify, manage, and secure a running process.

While users interact with processes through applications like Task Manager, the Windows kernel interacts directly with their EPROCESS structures.

---

# Why EPROCESS Matters

Every process—including system services, user applications, and malware—has an associated EPROCESS structure.

Security professionals study EPROCESS because it contains:

- Process identity
- Security context
- Memory information
- Parent-child relationships
- Handle tables
- Loaded modules
- Execution state

Many offensive and defensive techniques rely on understanding or manipulating EPROCESS.

---

# Process Architecture

```
Application

      │

Windows API

      │

Kernel

      │

EPROCESS

      │

Process Resources
```

The application is simply the visible representation.

The kernel manages the actual process through EPROCESS.

---

# What is EPROCESS?

EPROCESS is a kernel-mode data structure created whenever a process starts.

It stores metadata describing the process.

Think of it as the kernel's "profile" for every running process.

---

# Simplified EPROCESS Structure

```
EPROCESS
│
├── Process ID (PID)
├── Parent Process
├── Image File Name
├── Security Token
├── Handle Table
├── Virtual Address Space
├── Thread List
├── Loaded Modules
├── Object Table
├── Session Information
└── Process State
```

The real structure contains hundreds of fields, but these are the most security-relevant.

---

# Process Identifier (PID)

Every process receives a unique Process ID.

Example:

```
explorer.exe
PID: 3240
```

The PID uniquely identifies the process while it is running.

---

## Security Relevance

Attackers enumerate PIDs to:

- Locate privileged processes
- Inject code
- Dump memory
- Target specific applications

---

# Image File Name

Stores the executable associated with the process.

Example:

```
chrome.exe

notepad.exe

lsass.exe
```

---

## Security Relevance

Malware often:

- Masquerades as legitimate processes
- Uses similar names
- Creates fake system processes

Example:

```
explorer.exe

expl0rer.exe
```

---

# Parent Process

Every process is created by another process.

Example:

```
explorer.exe
        │
        ▼
cmd.exe
        │
        ▼
powershell.exe
```

This relationship is stored inside EPROCESS.

---

## Security Relevance

Attackers abuse parent-child relationships through:

- Parent PID Spoofing
- Suspicious process trees
- Living-off-the-Land attacks

EDR solutions heavily analyze process ancestry.

---

# Security Token

Each process owns a security token.

The token contains:

- User SID
- Group memberships
- Privileges
- Integrity Level

Windows uses this token whenever the process accesses protected resources.

---

## Security Relevance

Privilege escalation often targets process tokens.

Common techniques:

- Token stealing
- Token duplication
- Token impersonation

If a process acquires a SYSTEM token, it effectively becomes SYSTEM.

---

# Handle Table

Each process maintains a table of handles.

Example:

```
Handle

↓

File

↓

Registry Key

↓

Mutex

↓

Another Process
```

Handles allow the process to access kernel objects.

---

## Security Relevance

Attackers abuse handles to:

- Open privileged processes
- Duplicate handles
- Access protected resources

---

# Virtual Address Space

Every process receives its own isolated virtual memory.

Contains:

- Executable code
- DLLs
- Heap
- Stack
- Loaded libraries

Windows prevents processes from directly reading each other's memory.

---

## Security Relevance

Many attacks target process memory:

- DLL Injection
- Reflective Loading
- Process Hollowing
- Credential Dumping

---

# Thread List

A process contains one or more threads.

Each thread is represented internally by an ETHREAD structure.

Example:

```
Process

│

├── Thread 1

├── Thread 2

├── Thread 3
```

Threads perform the actual execution.

---

# Loaded Modules

The process keeps track of loaded modules.

Examples:

```
kernel32.dll

ntdll.dll

user32.dll

advapi32.dll
```

---

## Security Relevance

Attackers inject malicious DLLs or modify existing ones.

Examples:

- DLL Injection
- DLL Search Order Hijacking
- Reflective DLL Loading

---

# Object Table

Processes interact with Windows objects through an object table.

Objects include:

- Files
- Processes
- Events
- Registry Keys
- Tokens

The Object Manager manages these relationships.

---

# Session Information

Processes belong to Windows sessions.

Example:

```
Session 0

↓

Services

Session 1

↓

Logged-in User
```

---

## Security Relevance

Session isolation prevents normal users from interacting with system services.

---

# Process States

A process transitions through several states.

```
Created

↓

Ready

↓

Running

↓

Waiting

↓

Terminated
```

Windows updates the EPROCESS structure during each transition.

---

# Relationship with ETHREAD

```
EPROCESS

│

├── ETHREAD

├── ETHREAD

├── ETHREAD
```

EPROCESS manages the process.

ETHREAD manages execution.

---

# Relationship with the Executive

```
Windows Executive

        │

Process Manager

        │

EPROCESS

        │

ETHREAD

        │

Scheduler
```

The Process Manager creates and maintains EPROCESS structures.

---

# Common Offensive Techniques

Attackers rarely modify EPROCESS directly.

Instead, they abuse information stored within it.

Examples include:

### Process Injection

Inject malicious code into another process.

---

### Process Hollowing

Replace legitimate process memory with malicious code.

---

### Token Theft

Steal another process's security token.

---

### Handle Duplication

Reuse privileged handles from another process.

---

### Parent PID Spoofing

Create misleading process trees.

---

### DKOM (Direct Kernel Object Manipulation)

Advanced rootkits may directly modify EPROCESS structures.

Examples:

- Hide processes
- Remove processes from active lists
- Evade security tools

Modern Windows includes protections against many DKOM attacks.

---

# Defensive Perspective

EDR and antivirus solutions monitor EPROCESS to detect:

- Suspicious parent-child relationships
- Token abuse
- Memory injection
- Hidden processes
- Privilege escalation
- Process hollowing

Most behavioral detection begins with process metadata.

---

# Pentester Perspective

During an assessment, a pentester is interested in:

- High-privilege processes
- Process ancestry
- Running services
- Token ownership
- Injected modules
- Handle permissions

Understanding EPROCESS helps explain how Windows tracks and secures every process.

---

# Key Takeaways

- Every running process has an EPROCESS structure.
- EPROCESS is the kernel's representation of a process.
- It stores process identity, security context, memory information, and execution state.
- Process tokens determine the privileges of the process.
- Most process-based attacks abuse information stored in EPROCESS rather than attacking the structure itself.
- Security tools rely heavily on EPROCESS metadata to detect malicious behavior.

---

# Key Insight

> EPROCESS is the kernel's blueprint for every running process. It defines not only what a process is, but also what it can access, how it executes, and how Windows enforces security around it. Understanding EPROCESS is fundamental to both Windows internals and advanced offensive security.
