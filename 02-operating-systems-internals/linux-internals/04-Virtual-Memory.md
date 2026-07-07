# Virtual Memory (Security Perspective)

## Overview

Virtual Memory is a memory management technique that gives each process its own isolated address space, regardless of the amount of physical RAM installed.

From a security perspective:

> Virtual Memory isolates processes, protects kernel memory, and enables modern security mechanisms such as ASLR, NX, and memory protection. Many exploitation techniques revolve around bypassing or abusing virtual memory.

Understanding Virtual Memory is essential for exploit development, malware analysis, privilege escalation, and memory forensics.

---

# What is Virtual Memory?

Virtual Memory provides every process with its own private memory space.

Instead of accessing physical RAM directly:

```
Application

↓

Virtual Address

↓

Memory Management Unit (MMU)

↓

Physical Address
```

The translation is handled by the kernel and the CPU.

---

# High-Level Architecture

```
Process

↓

Virtual Address Space

↓

Page Tables

↓

Physical Memory (RAM)

↓

Storage (Swap)
```

Every process believes it owns the entire address space.

---

# Why Virtual Memory Exists

Virtual Memory provides:

- Process isolation
- Memory protection
- Efficient RAM utilization
- Shared libraries
- Larger address spaces
- Secure execution

Without Virtual Memory, one process could overwrite another process or even the operating system.

---

# Virtual Address Space

Each process receives its own virtual memory.

Typical layout:

```
High Addresses
+----------------------+
| Kernel Space         |
+----------------------+
| Shared Libraries     |
+----------------------+
| Heap                 |
+----------------------+
| Unused               |
+----------------------+
| Stack                |
+----------------------+
| Code (.text)         |
+----------------------+
Low Addresses
```

Each process has an independent layout.

---

# Memory Segments

## Code Segment (.text)

Contains:

- Executable instructions
- Read-only program code

Security relevance:

Attackers attempt to execute malicious code here.

---

## Data Segment

Stores:

- Global variables
- Static variables

---

## Heap

Used for:

- Dynamic memory allocation
- malloc()
- calloc()

Security relevance:

Common target for:

- Heap overflow
- Use-after-free
- Heap spraying

---

## Stack

Stores:

- Function calls
- Local variables
- Return addresses

Security relevance:

Primary target for:

- Stack overflow
- Return-Oriented Programming (ROP)

---

## Shared Libraries

Libraries mapped into process memory.

Examples:

- libc.so
- libpthread.so

Security relevance:

- Library hijacking
- GOT overwrite
- PLT abuse

---

# Memory Paging

Linux divides memory into fixed-size pages.

```
Virtual Memory

↓

Pages

↓

Physical Frames
```

Typical page size:

```
4 KB
```

---

# Page Tables

Page tables translate:

```
Virtual Address

↓

Physical Address
```

Managed by:

- Kernel
- MMU

Applications never access physical memory directly.

---

# Swap Space

When RAM becomes full:

```
RAM

↓

Swap Partition / Swap File
```

Inactive pages are moved to disk.

Security relevance:

Sensitive data may remain inside swap.

---

# Memory Protection

Each page has permissions.

| Permission | Meaning |
|------------|---------|
| Read (R) | Can read |
| Write (W) | Can modify |
| Execute (X) | Can execute |

Examples:

```
RW-

R-X

RWX
```

These permissions are enforced by the kernel.

---

# Security Mechanisms

## ASLR (Address Space Layout Randomization)

Randomizes memory locations.

Protects against:

- Buffer Overflow
- ROP
- Memory corruption

---

## NX (No-eXecute Bit)

Marks memory as:

```
Non-executable
```

Protects against:

- Shellcode execution
- Stack execution
- Heap execution

---

## PIE (Position Independent Executable)

Allows executables to load at random addresses.

Improves ASLR effectiveness.

---

## RELRO

Protects:

- GOT
- Dynamic linking structures

Makes exploitation more difficult.

---

# Common Memory Attacks

| Attack | Target |
|----------|---------|
| Stack Overflow | Stack |
| Heap Overflow | Heap |
| Use-After-Free | Heap |
| Double Free | Heap |
| Memory Corruption | Virtual Memory |
| ROP | Stack |
| Shellcode Injection | Writable memory |

---

# Process Isolation

Each process has:

- Private stack
- Private heap
- Private data
- Private code mappings

One process cannot directly access another process's memory.

The kernel enforces this isolation.

---

# Shared Memory

Linux also supports shared memory.

```
Process A

↓

Shared Memory

↑

Process B
```

Useful for:

- IPC
- Performance

Security relevance:

Improper permissions may expose sensitive information.

---

# Offensive Perspective

Attackers analyze:

- Memory layout
- ASLR status
- NX protection
- Stack addresses
- Heap allocations
- Shared libraries

Goals include:

- Arbitrary code execution
- Information disclosure
- Privilege escalation
- Bypassing memory protections

---

# Defensive Perspective

Security teams rely on:

- ASLR
- NX
- PIE
- RELRO
- Stack canaries
- Memory-safe programming
- Kernel memory protections

These mechanisms significantly increase exploitation difficulty.

---

# Memory Layout Summary

| Region | Purpose | Security Relevance |
|---------|----------|--------------------|
| .text | Executable code | Code execution |
| Data | Global variables | Data integrity |
| Heap | Dynamic allocation | Heap exploits |
| Stack | Function calls | Stack overflows |
| Shared Libraries | External code | Library hijacking |
| Kernel Space | OS memory | Privilege boundary |

---

# Why Virtual Memory Matters

Understanding Virtual Memory helps security professionals:

- Analyze exploit techniques
- Understand memory corruption
- Investigate malware
- Perform memory forensics
- Develop secure software
- Understand modern exploit mitigations

---

# Key Takeaways

- Every Linux process has its own isolated virtual address space.
- Virtual addresses are translated to physical memory through page tables.
- Memory is divided into code, data, heap, stack, and shared library regions.
- The kernel enforces page permissions and process isolation.
- Security mechanisms such as ASLR, NX, PIE, and RELRO rely on Virtual Memory.
- Many modern exploitation techniques focus on bypassing virtual memory protections.

---

# Key Insight

> Virtual Memory is more than a memory management feature it is a fundamental security boundary. It isolates processes, protects kernel memory, and provides the foundation for nearly every modern memory protection mechanism in Linux.
