# Handles (Windows Internals - Security Perspective)

## Overview

A **Handle** is a unique identifier that allows a process to access a Windows kernel object.

From a security perspective:

> Handles are the gateway between user-mode applications and kernel objects. If an attacker can obtain a privileged handle, they may gain access to sensitive resources without directly bypassing security mechanisms.

Every time an application opens a file, creates a process, accesses a registry key, or communicates with another process, it does so through handles.

---

# Why Handles Exist

Applications cannot directly access kernel objects because:

- Kernel memory is protected.
- User applications run in User Mode.
- Direct access would compromise system stability and security.

Instead, Windows provides **handles** as secure references to kernel objects.

---

# Handle Architecture

```
Application

      │

Handle (0x000001F4)

      │

Handle Table

      │

Object Manager

      │

Kernel Object
```

Applications never receive direct pointers to kernel objects.

Instead, they receive handles managed by the Object Manager.

---

# What is a Handle?

A handle is:

- A unique integer value
- Valid only inside one process
- A reference to a kernel object
- Associated with specific access rights

Example:

```
Handle: 0x000001F4

↓

File Object
```

---

# Handle Tables

Each process owns its own **Handle Table**.

Example:

```
Process

│

├── Handle 0x100 → File
├── Handle 0x104 → Registry Key
├── Handle 0x108 → Event
├── Handle 0x10C → Process
└── Handle 0x110 → Thread
```

Windows uses the handle table to resolve handles into actual kernel objects.

---

# Kernel Objects That Use Handles

Handles can reference many object types.

Examples include:

- Processes
- Threads
- Files
- Registry Keys
- Events
- Mutexes
- Semaphores
- Tokens
- Named Pipes
- Devices

Almost every securable Windows object can be accessed through a handle.

---

# Handle Creation

When an application requests access to an object:

```
OpenProcess()

↓

Object Manager

↓

Security Check

↓

Handle Created
```

If permission is granted, Windows returns a handle.

Otherwise:

```
Access Denied
```

---

# Access Rights

Every handle contains specific permissions.

Examples:

### File Handle

- Read
- Write
- Execute
- Delete

---

### Process Handle

- Read Memory
- Write Memory
- Create Thread
- Suspend Process
- Terminate Process

---

### Registry Handle

- Read Key
- Write Key
- Delete Key

---

## Security Importance

Two handles may reference the same object but have different permissions.

Example:

```
Handle A

↓

Read Only

Handle B

↓

Read + Write
```

The handle determines what operations are allowed.

---

# Handle Lifetime

A handle exists until:

- The application closes it
- The process terminates
- Windows automatically cleans it up

Proper applications always close unused handles.

---

# Reference Counting

Every object tracks how many handles reference it.

Example:

```
Object

Reference Count = 3
```

When a handle closes:

```
Reference Count = 2
```

When the count reaches zero:

```
Object Deleted
```

This prevents deleting objects that are still in use.

---

# Common APIs

Windows provides APIs to manage handles.

Examples:

```
CreateFile()

OpenProcess()

OpenThread()

OpenEvent()

OpenMutex()

CloseHandle()
```

Most Windows API calls return a handle.

---

# Handle Inheritance

Child processes may inherit handles from parent processes.

Example:

```
Parent Process

↓

Inherited Handle

↓

Child Process
```

---

## Security Relevance

Improper handle inheritance may expose sensitive resources to child processes.

---

# Handle Duplication

Windows allows one process to duplicate handles into another process.

```
Process A

↓

DuplicateHandle()

↓

Process B
```

---

## Security Relevance

Handle duplication is widely abused during post-exploitation.

---

# Process Handles

Opening another process requires a process handle.

Example:

```
OpenProcess()

↓

Handle

↓

Target Process
```

Permissions determine allowed operations.

---

## Common Access Rights

Examples include:

- PROCESS_VM_READ
- PROCESS_VM_WRITE
- PROCESS_VM_OPERATION
- PROCESS_CREATE_THREAD
- PROCESS_TERMINATE

---

## Security Importance

Attackers often seek process handles with high privileges.

These enable:

- Memory injection
- Credential dumping
- Process manipulation

---

# Thread Handles

Threads also have handles.

Used for:

- Suspend
- Resume
- Read Context
- Modify Context

---

## Security Relevance

Thread handles enable:

- Thread hijacking
- APC injection
- Context manipulation

---

# Token Handles

Security tokens are kernel objects accessed through handles.

A token handle allows:

- Reading privileges
- Impersonation
- Duplication

---

## Security Importance

Token handles are frequently abused during privilege escalation.

---

# File Handles

Opening a file returns a file handle.

```
Application

↓

CreateFile()

↓

Handle

↓

File Object
```

Applications use the handle for all file operations.

---

# Registry Handles

Registry operations require registry handles.

Examples:

- Read values
- Modify keys
- Delete entries

---

# Common Offensive Techniques

## Handle Enumeration

Attackers enumerate handles to identify:

- High-privilege processes
- Open files
- Security tokens
- Sensitive objects

---

## Handle Duplication

Duplicate a privileged process handle.

Possible uses:

- Read memory
- Inject code
- Escalate privileges

---

## Process Injection

Obtain a process handle with:

```
PROCESS_VM_WRITE

PROCESS_CREATE_THREAD
```

Then inject malicious code.

---

## Token Theft

Duplicate token handles belonging to SYSTEM processes.

---

## LSASS Access

Credential dumping requires opening a handle to:

```
lsass.exe
```

If access is granted, attackers may extract credentials.

---

# Defensive Perspective

Modern EDR solutions monitor:

- OpenProcess()
- DuplicateHandle()
- Handle enumeration
- Handle access to LSASS
- Unusual handle creation
- Cross-process handle requests

Abnormal handle activity is a common indicator of compromise.

---

# Pentester Perspective

When assessing a Windows system, pentesters ask:

- Can I open privileged process handles?
- Can I duplicate existing handles?
- Can I access LSASS?
- Can I obtain SYSTEM token handles?
- Are handle permissions overly permissive?

Many privilege escalation techniques begin with obtaining the right handle.

---

# Relationship with Other Components

```
Application

      │

Windows API

      │

Handle

      │

Handle Table

      │

Object Manager

      │

Kernel Object
```

Handles provide the controlled interface between user-mode applications and kernel resources.

---

# Common Attack Techniques

| Technique | Handle Used |
|-----------|-------------|
| Process Injection | Process Handle |
| Credential Dumping | Process Handle (LSASS) |
| Token Theft | Token Handle |
| Thread Hijacking | Thread Handle |
| Registry Persistence | Registry Handle |
| File Manipulation | File Handle |

---

# Key Takeaways

- Handles are references to Windows kernel objects.
- Every process maintains its own handle table.
- Handles define what operations are permitted on an object.
- Access rights are enforced when the handle is created.
- Handle duplication is a common post-exploitation technique.
- Many offensive techniques rely on obtaining privileged handles rather than bypassing Windows security directly.

---

# Key Insight

> Handles are not the objects themselve, they are controlled references managed by the Object Manager. In Windows security, controlling a privileged handle often provides the same practical capabilities as controlling the underlying object, making handle abuse a common path to privilege escalation and code execution.
