# Executive & Kernel (Security Perspective)

## Overview

The **Windows Kernel** and the **Windows Executive** form the core of the operating system.

Together, they are responsible for managing hardware resources, enforcing security, handling processes, managing memory, and providing essential operating system services.

From a security perspective:

> Every process, privilege, memory allocation, and security decision ultimately passes through the Kernel and Executive.

Understanding these components helps explain how Windows controls execution—and how attackers attempt to abuse that control.

---

# Windows Kernel vs Executive

Although often treated as one component, they have different responsibilities.

| Component | Primary Responsibility |
|-----------|------------------------|
| Kernel | Low-level system control |
| Executive | High-level operating system services |

Think of them as:

```
Applications
      │
      ▼
Windows Executive
      │
      ▼
Windows Kernel
      │
      ▼
Hardware
```

The Executive provides operating system functionality.

The Kernel performs the low-level execution.

---

# Windows Kernel

The Kernel is responsible for the most fundamental operations inside Windows.

Main responsibilities include:

- CPU scheduling
- Thread execution
- Interrupt handling
- Synchronization
- Exception handling
- Context switching

---

## CPU Scheduling

The kernel decides:

- Which thread executes
- When it executes
- For how long

This allows Windows to run thousands of processes efficiently.

---

## Interrupt Handling

Hardware devices interrupt the CPU whenever they require attention.

Examples:

- Keyboard input
- Mouse movement
- Disk operations
- Network packets

The kernel processes these interrupts and dispatches them to the appropriate drivers.

---

## Context Switching

Modern CPUs execute only one thread per logical core at a time.

The kernel rapidly switches between threads by saving and restoring execution states.

This creates the illusion of simultaneous execution.

---

## Synchronization

Multiple processes often access shared resources.

The kernel provides synchronization mechanisms to prevent:

- Race conditions
- Memory corruption
- Resource conflicts

---

# Windows Executive

The Executive sits above the kernel and provides higher-level operating system functionality.

Instead of communicating directly with hardware, applications interact with Executive components.

---

# Major Executive Components

The Executive consists of several managers.

Each manages a specific part of the operating system.

---

## 1. Object Manager

Windows represents nearly everything as an object.

Examples:

- Files
- Processes
- Threads
- Registry keys
- Events
- Mutexes

The Object Manager:

- Creates objects
- Tracks objects
- Controls object access
- Deletes unused objects

### Security Relevance

Access control decisions begin here.

Attackers often interact with objects during:

- Handle duplication
- Token stealing
- Process injection

---

## 2. Process Manager

Responsible for:

- Creating processes
- Creating threads
- Managing process lifecycles
- Process termination

### Security Relevance

Every malware sample ultimately becomes a process.

Attackers abuse:

- Process creation
- Process injection
- Parent PID spoofing
- Process hollowing

---

## 3. Memory Manager

Responsible for:

- Virtual memory
- Physical memory
- Paging
- Memory protection
- Address translation

### Security Relevance

Many attacks target memory.

Examples:

- Buffer overflows
- DLL injection
- Credential dumping
- Reflective loading

---

## 4. Security Reference Monitor (SRM)

The Security Reference Monitor enforces Windows security policies.

Responsibilities include:

- Access checks
- Permission validation
- Privilege verification
- Security auditing

Whenever a process requests access to an object, SRM decides whether access is allowed.

### Security Relevance

SRM enforces:

- NTFS permissions
- Process permissions
- Token privileges
- Object security descriptors

---

## 5. I/O Manager

Handles communication between applications and hardware.

Responsibilities:

- File operations
- Device requests
- Driver communication
- Network I/O

Applications never communicate directly with hardware.

Everything flows through the I/O Manager.

---

## 6. Cache Manager

Improves system performance by caching frequently accessed data.

Instead of reading from disk every time, Windows retrieves data from memory whenever possible.

---

## 7. Plug and Play (PnP) Manager

Responsible for:

- Detecting new hardware
- Loading drivers
- Managing hardware resources

---

## 8. Power Manager

Controls:

- Sleep
- Hibernate
- Battery management
- Power states

Mostly important for system stability rather than offensive security.

---

# Executive Architecture

```
Windows Executive
│
├── Object Manager
├── Process Manager
├── Memory Manager
├── Security Reference Monitor
├── I/O Manager
├── Cache Manager
├── Plug and Play Manager
└── Power Manager
```

Each component provides specialized operating system functionality.

---

# How Applications Interact

Typical execution flow:

```
Application

      │

Windows API

      │

System Call

      │

Executive Manager

      │

Kernel

      │

Hardware
```

Applications never communicate directly with hardware.

---

# Security Boundaries

The Executive helps enforce several trust boundaries:

- User vs Administrator
- User Mode vs Kernel Mode
- Process isolation
- Memory protection
- Object permissions

Attackers continuously attempt to cross these boundaries.

---

# Why Attackers Care

The Executive contains many valuable attack targets.

Examples:

| Component | Common Abuse |
|-----------|--------------|
| Process Manager | Process injection |
| Memory Manager | Code injection |
| Object Manager | Handle duplication |
| SRM | Privilege abuse |
| I/O Manager | Driver exploitation |

---

# Pentester Perspective

A pentester rarely attacks the Kernel directly.

Instead, they ask:

- Can I inject into another process?
- Can I obtain a privileged token?
- Can I access protected objects?
- Can I exploit a vulnerable driver?
- Can I abuse memory protections?

Understanding the Executive explains **why** these attacks work.

---

# Malware Perspective

Modern malware frequently abuses Executive components.

Common techniques include:

- Process Hollowing
- DLL Injection
- Token Impersonation
- Handle Duplication
- Direct System Calls
- Memory Allocation Abuse

These techniques rely on Executive services rather than breaking Windows itself.

---

# Relationship Between Executive and Kernel

| Executive | Kernel |
|------------|---------|
| High-level OS services | Low-level execution |
| Object management | Thread scheduling |
| Process management | Interrupt handling |
| Memory management | Context switching |
| Security decisions | CPU control |

Both components work together to provide a secure and stable operating system.

---

# Key Takeaways

- The Kernel controls low-level execution and hardware interaction.
- The Executive provides core operating system services.
- Security decisions pass through Executive managers.
- Applications interact with Windows through Executive components.
- Most privilege escalation and process manipulation techniques involve Executive functionality.

---

# Key Insight

> The Kernel executes Windows, but the Executive manages it. Understanding both reveals how Windows enforces trust—and how attackers attempt to bypass it through processes, memory, objects, and security mechanisms.
