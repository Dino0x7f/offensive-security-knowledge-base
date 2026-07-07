# Object Manager (Security Perspective)

## Overview

The **Object Manager** is one of the core components of the Windows Executive responsible for creating, managing, securing, and deleting operating system objects.

From a security perspective:

> Almost everything inside Windows is represented as an object. Controlling access to these objects is one of the primary ways Windows enforces security.

Without the Object Manager, Windows would have no standardized way to manage processes, files, events, threads, or security.

---

# Why the Object Manager Exists

Instead of every subsystem implementing its own resource management, Windows uses a unified object-based architecture.

This provides:

- Consistent resource management
- Centralized security enforcement
- Reference counting
- Handle management
- Namespace organization

---

# What is an Object?

An object is a data structure managed by the Windows kernel that represents a system resource.

Examples include:

- Processes
- Threads
- Files
- Directories
- Registry Keys
- Events
- Mutexes
- Semaphores
- Tokens
- Drivers
- Devices
- Named Pipes

Everything is accessed through objects.

---

# Object Architecture

```
Application
      │
      ▼
Handle
      │
      ▼
Object Manager
      │
      ▼
Kernel Object
```

Applications never access kernel objects directly.

Instead, they use **handles** provided by the Object Manager.

---

# Handles

A **Handle** is an identifier that references an object.

Think of it as a secure "pointer" that applications use instead of accessing objects directly.

Example:

```
Process

     │

Handle (0x4A8)

     │

Kernel Process Object
```

Applications work with handles, while Windows resolves them to the actual kernel objects.

---

# Object Lifecycle

Every object follows a lifecycle.

```
Create Object
      │
      ▼
Assign Security Descriptor
      │
      ▼
Generate Handle
      │
      ▼
Application Uses Object
      │
      ▼
Reference Count Decreases
      │
      ▼
Object Deleted
```

The Object Manager automatically tracks when objects are no longer needed.

---

# Reference Counting

Each object maintains a reference count.

Whenever:

- A process opens the object
- A driver uses the object
- Another component references it

The count increases.

When references disappear:

- The count decreases.

When it reaches zero:

- Windows automatically destroys the object.

---

# Object Namespace

Windows organizes many objects inside an internal namespace.

Examples:

```
\Device
\Driver
\BaseNamedObjects
\RPC Control
\KnownDlls
```

This hierarchy allows Windows to locate and manage objects efficiently.

---

# Security Descriptors

Every securable object contains a **Security Descriptor**.

It defines:

- Owner
- Group
- Permissions
- Access Control Lists (ACLs)
- Audit rules

Whenever a process requests access, Windows checks the object's security descriptor.

---

# Access Check Process

When a process requests an object:

```
Application

      │

Open Object Request

      │

Object Manager

      │

Security Reference Monitor

      │

Permission Granted?
      │
 ┌────┴────┐
 │         │
Yes       No
 │         │
 ▼         ▼
Handle   Access Denied
```

The Object Manager works closely with the Security Reference Monitor (SRM) to enforce access control.

---

# Types of Kernel Objects

Some common Windows objects include:

| Object | Purpose |
|---------|----------|
| Process | Running program |
| Thread | Unit of execution |
| File | Stored data |
| Event | Synchronization |
| Mutex | Mutual exclusion |
| Semaphore | Resource synchronization |
| Token | Security identity |
| Registry Key | System configuration |
| Device | Hardware abstraction |
| Driver | Kernel driver |

---

# Process Objects

Every running program has a Process Object.

Contains:

- Process ID
- Security Token
- Memory information
- Open handles
- Thread list

---

## Security Relevance

Attackers target process objects for:

- Process injection
- Handle duplication
- Token stealing
- Process hollowing

---

# Thread Objects

Each process contains one or more thread objects.

Responsible for:

- CPU execution
- Scheduling
- Context switching

---

# Token Objects

One of the most important security objects.

A Token contains:

- User SID
- Group memberships
- Privileges
- Integrity Level

Whenever Windows performs an access check, it compares:

```
User Token

VS

Object Security Descriptor
```

---

## Security Relevance

Privilege escalation often involves:

- Token theft
- Token impersonation
- Token duplication

---

# File Objects

Opening a file creates a File Object.

Contains:

- File location
- Access mode
- Current offset
- Security descriptor

Applications interact with the File Object rather than the physical disk.

---

# Event Objects

Events allow synchronization between processes.

Examples:

- Signal completion
- Wake waiting threads
- Coordinate execution

---

# Mutex Objects

Mutexes ensure that only one thread accesses a shared resource at a time.

Useful for:

- Preventing race conditions
- Resource locking

---

# Named Objects

Some objects receive names inside the Object Namespace.

Examples:

```
Global\MyMutex

Local\SharedMemory

\BaseNamedObjects
```

Named objects allow different processes to communicate.

---

# Handle Tables

Each process maintains its own Handle Table.

```
Process

│

├── Handle 0x100

├── Handle 0x124

├── Handle 0x220

└── Handle 0x350
```

Each handle points to a kernel object.

---

# Common Attacks Against the Object Manager

Attackers abuse object management rather than attacking the Object Manager itself.

Examples include:

### Handle Duplication

Duplicate another process's handles to gain access.

---

### Token Theft

Obtain privileged security tokens.

---

### Process Injection

Open another process object and inject code.

---

### Handle Enumeration

Discover valuable handles owned by privileged processes.

---

### Named Object Abuse

Interact with existing shared objects for privilege escalation or persistence.

---

# Pentester Perspective

When enumerating a Windows host, pentesters ask:

- Can I open privileged process handles?
- Can I duplicate handles?
- Can I access privileged tokens?
- Are object permissions misconfigured?
- Can I abuse named objects?

The Object Manager often becomes part of privilege escalation chains.

---

# Malware Perspective

Modern malware frequently abuses kernel objects.

Examples:

- Token impersonation
- Process injection
- Named mutexes
- Handle duplication
- Shared memory objects

Rather than creating new mechanisms, malware leverages existing Windows object management.

---

# Relationship with Other Components

```
Application
      │
      ▼
Object Manager
      │
      ├── Process Manager
      ├── Memory Manager
      ├── I/O Manager
      ├── Security Reference Monitor
      └── Handle Tables
```

The Object Manager acts as the central coordinator for Windows objects.

---

# Key Takeaways

- Everything in Windows is represented as an object.
- Applications access objects through handles.
- Every object has a security descriptor.
- The Object Manager works with the Security Reference Monitor to enforce access control.
- Token, process, and file objects are common targets during attacks.
- Most privilege escalation techniques involve abusing access to existing objects rather than breaking the Object Manager itself.

---

# Key Insight

> The Object Manager is the foundation of Windows resource management. It doesn't execute code or enforce policy alone it provides the secure abstraction layer through which every important system resource is created, accessed, and protected.
