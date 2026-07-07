# Virtual Memory (Windows Internals - Security Perspective)

## Overview

**Virtual Memory** is a memory management technique that gives every process its own isolated address space, regardless of the amount of physical RAM available.

From a security perspective:

> Virtual Memory is one of Windows' strongest security boundaries. It isolates processes, protects memory, and makes many modern exploit mitigations possible.

Without Virtual Memory, any process could read or modify another process's memory directly.

---

# Why Virtual Memory Exists

Virtual Memory provides several benefits:

- Process isolation
- Efficient memory management
- Protection between applications
- Large address spaces
- Support for memory-mapped files
- Foundation for modern exploit mitigations

---

# Physical Memory vs Virtual Memory

## Physical Memory

Physical memory refers to the actual RAM installed in the computer.

Example:

```
RAM

+----------------------+
| Physical Memory      |
+----------------------+
```

---

## Virtual Memory

Virtual memory is the address space that each process sees.

```
Process A

0x00000000
     │
     ▼
Virtual Memory

Process B

0x00000000
     │
     ▼
Virtual Memory
```

Both processes believe they own the same addresses, but Windows maps them to different physical memory locations.

---

# Address Translation

Applications never access RAM directly.

Instead:

```
Application

↓

Virtual Address

↓

Memory Manager

↓

Page Tables

↓

Physical Address

↓

RAM
```

Windows performs this translation automatically.

---

# Virtual Address Space

Each process receives its own private virtual address space.

Simplified view:

```
High Addresses

+----------------------+
| Kernel Space         |
+----------------------+
| User Space           |
+----------------------+

Low Addresses
```

---

# User Space vs Kernel Space

## User Space

Contains:

- Application code
- Heap
- Stack
- DLLs

Accessible only by the owning process.

---

## Kernel Space

Contains:

- Windows Kernel
- Drivers
- Executive
- System objects

Accessible only in Kernel Mode.

---

## Security Importance

This separation prevents user applications from directly modifying kernel memory.

---

# Memory Pages

Windows divides memory into small blocks called **Pages**.

Typical page size:

```
4 KB
```

Each page has its own protection settings.

---

# Page Tables

Windows keeps page tables that map:

```
Virtual Address

↓

Physical Address
```

Applications never see these tables.

The CPU and Memory Manager use them during execution.

---

# Memory Allocation

Applications request memory using Windows APIs.

Example:

```
Application

↓

VirtualAlloc()

↓

Memory Manager

↓

Virtual Memory Assigned
```

The application receives virtual addresses instead of physical ones.

---

# Paging

If RAM becomes full:

Windows moves inactive pages to disk.

```
RAM

↓

Page File

↓

Disk
```

This process is called **Paging**.

---

# Page File

The Page File (`pagefile.sys`) extends available memory.

Advantages:

- More virtual memory
- Better memory utilization

Disadvantages:

- Slower than RAM
- Potential forensic evidence

---

## Security Relevance

Sensitive data may remain inside the page file, including:

- Passwords
- Encryption keys
- Documents
- Browser sessions

Forensic investigators often analyze pagefile.sys.

---

# Memory Protection

Each page has protection flags.

Common permissions:

| Permission | Description |
|------------|-------------|
| Read | Can read |
| Write | Can modify |
| Execute | Can run code |
| No Access | Completely blocked |

---

# DEP (Data Execution Prevention)

DEP prevents execution from data pages.

Example:

```
Stack

Read

Write

Execute ❌
```

This blocks many buffer overflow attacks.

---

## Security Relevance

Attackers must bypass DEP using techniques such as:

- ROP
- Return-to-libc
- JOP

---

# ASLR (Address Space Layout Randomization)

Windows randomizes memory locations every time a process starts.

Randomized areas include:

- DLLs
- Heap
- Stack
- Executables

---

## Security Relevance

Without ASLR:

Attackers know exactly where code resides.

With ASLR:

Memory addresses become unpredictable.

---

# Heap

The Heap stores dynamically allocated memory.

Used for:

- Objects
- Strings
- Buffers
- Dynamic data

---

## Security Relevance

Attackers target the heap using:

- Heap Overflow
- Heap Spraying
- Heap Corruption

---

# Stack

Each thread has its own stack.

Stores:

- Function calls
- Local variables
- Return addresses

---

## Security Relevance

Classic attacks include:

- Stack Buffer Overflow
- Return Address Overwrite
- Stack Pivoting

---

# Memory-Mapped Files

Windows can map files directly into virtual memory.

```
Disk File

↓

Memory Mapping

↓

Virtual Address Space
```

Advantages:

- Faster access
- Shared memory
- Efficient I/O

---

# Shared Memory

Some pages may be shared between processes.

Examples:

- Shared DLLs
- Shared sections
- IPC mechanisms

---

# Copy-on-Write

Initially, processes may share memory pages.

When one process modifies a shared page:

```
Shared Page

↓

Write

↓

Private Copy Created
```

This saves memory while maintaining isolation.

---

# Common Offensive Techniques

## Process Injection

Allocate executable memory inside another process.

```
VirtualAllocEx()

↓

WriteProcessMemory()

↓

CreateRemoteThread()
```

---

## Reflective DLL Injection

Load DLLs directly into memory without touching disk.

---

## Process Hollowing

Replace legitimate process memory with malicious code.

---

## Memory Scanning

Search memory for:

- Credentials
- Tokens
- API keys
- Secrets

---

## Memory Dumping

Extract process memory for offline analysis.

Common targets:

- LSASS
- Browsers
- Password managers

---

# Defensive Perspective

Security solutions monitor:

- Executable memory allocations
- RWX memory pages
- Memory injections
- Suspicious page protections
- Unbacked executable memory

Modern EDR products rely heavily on memory analysis.

---

# Pentester Perspective

When examining virtual memory, pentesters look for:

- Writable and executable pages
- Memory-resident malware
- Injected modules
- Loaded DLLs
- Credential storage
- Memory protection weaknesses

Most post-exploitation frameworks heavily interact with virtual memory.

---

# Relationship with Other Components

```
Application

↓

Virtual Address

↓

Memory Manager

↓

Page Tables

↓

Physical RAM

↓

CPU
```

The Memory Manager coordinates translation, protection, and allocation of virtual memory.

---

# Common Attack Techniques Related to Virtual Memory

| Technique | Purpose |
|-----------|---------|
| Process Injection | Execute code inside another process |
| Process Hollowing | Replace legitimate process memory |
| DLL Injection | Load malicious libraries |
| Memory Dumping | Extract credentials and secrets |
| Heap Exploitation | Corrupt heap structures |
| Stack Overflow | Redirect execution |
| ROP Chains | Bypass DEP |

---

# Key Takeaways

- Every process has its own isolated virtual address space.
- Virtual addresses are translated into physical addresses by the Memory Manager.
- Memory is divided into pages with individual protection settings.
- DEP and ASLR are built on top of Virtual Memory.
- Modern malware frequently abuses virtual memory for injection and evasion.
- Most advanced Windows exploitation techniques ultimately manipulate virtual memory.

---

# Key Insight

> Virtual Memory is more than a memory management feature it is one of Windows' primary security boundaries. It isolates processes, enforces memory protections, and underpins modern exploit mitigations. Understanding Virtual Memory is essential for both exploit development and defensive detection.
