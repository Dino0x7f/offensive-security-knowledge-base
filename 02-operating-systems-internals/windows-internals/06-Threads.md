# Threads (Windows Internals - Security Perspective)

## Overview

A **Thread** is the smallest unit of execution within a process.

While a process provides the resources (memory, handles, security context), **threads perform the actual work**.

From a security perspective:

> Attackers rarely target the process itself—they target the threads that execute code inside it.

Understanding threads is fundamental for learning process injection, malware execution, scheduling, and many Windows internals.

---

# What is a Thread?

A thread is an independent execution path inside a process.

Every process has **at least one thread** (the primary thread), but most applications create multiple threads to perform tasks concurrently.

Example:

```
Process (chrome.exe)

├── Thread 1 (UI)
├── Thread 2 (Rendering)
├── Thread 3 (Networking)
├── Thread 4 (Extensions)
└── Thread 5 (GPU)
```

The process owns resources.

The threads use those resources to execute instructions.

---

# Process vs Thread

| Process | Thread |
|----------|---------|
| Owns memory | Uses process memory |
| Owns handles | Shares handles |
| Owns security token | Uses process token |
| Contains threads | Executes instructions |
| Heavyweight | Lightweight |

---

# Why Threads Exist

Instead of creating multiple processes, applications create multiple threads because they:

- Use less memory
- Start faster
- Share resources
- Improve responsiveness
- Enable parallel execution

---

# Thread Components

Each thread has its own:

- Thread ID (TID)
- Stack
- CPU registers
- Program Counter (Instruction Pointer)
- Execution state
- Scheduling information

Shared with the process:

- Virtual memory
- Open handles
- Loaded DLLs
- Security token

---

# ETHREAD Structure

Internally, Windows represents every thread using an **ETHREAD** structure.

```
EPROCESS
│
├── ETHREAD
├── ETHREAD
├── ETHREAD
└── ETHREAD
```

While **EPROCESS** describes the process, **ETHREAD** describes each individual thread.

---

# Thread Lifecycle

A thread typically follows this lifecycle:

```
Created
    │
    ▼
Ready
    │
    ▼
Running
    │
    ▼
Waiting
    │
    ▼
Terminated
```

The Windows scheduler manages these transitions.

---

# Thread Scheduling

The Windows Kernel schedules **threads**, not processes.

The scheduler decides:

- Which thread runs
- When it runs
- On which CPU core
- For how long

Only one thread executes on a logical CPU core at any given moment.

---

# Context Switching

When switching between threads, Windows performs a **context switch**.

The kernel saves the current thread's execution state and restores another thread's state.

```
Thread A

↓

Save Registers

↓

Load Registers

↓

Thread B
```

Context switching enables multitasking.

---

# Thread Priorities

Windows assigns each thread a priority.

Higher-priority threads receive more CPU time.

Simplified priority levels:

- Low
- Below Normal
- Normal
- Above Normal
- High
- Real-Time

---

## Security Relevance

Attackers sometimes abuse priorities to:

- Keep malware responsive
- Starve security software
- Maintain persistence

---

# Thread Stack

Each thread has its own **stack**.

The stack stores:

- Function calls
- Local variables
- Return addresses

Example:

```
Thread

│

Stack

├── Function A
├── Function B
├── Function C
```

---

## Security Relevance

Many classic vulnerabilities target the stack.

Examples:

- Stack Buffer Overflow
- Return-Oriented Programming (ROP)
- Stack corruption

---

# Thread Local Storage (TLS)

Each thread has private storage for thread-specific data.

This prevents conflicts between threads using the same functions.

---

# Thread States

Common execution states include:

| State | Description |
|---------|-------------|
| Ready | Waiting for CPU |
| Running | Currently executing |
| Waiting | Waiting for an event |
| Suspended | Temporarily paused |
| Terminated | Execution finished |

---

# Multi-threaded Applications

Most modern applications create many threads.

Example:

```
Browser

├── UI Thread
├── Rendering Thread
├── JavaScript Thread
├── Network Thread
└── Audio Thread
```

Each performs different tasks simultaneously.

---

# Security Importance

Threads are common targets because they execute code.

Attackers frequently manipulate threads rather than entire processes.

---

# Common Offensive Techniques

## Remote Thread Injection

A new thread is created inside another process.

```
Attacker Process

↓

Create Remote Thread

↓

Victim Process

↓

Malicious Code Executes
```

Common in malware and red-team tooling.

---

## Thread Hijacking

Instead of creating a new thread, attackers reuse an existing one.

Process:

1. Suspend thread
2. Modify registers
3. Change execution address
4. Resume thread

Advantages:

- More stealthy
- Harder to detect

---

## APC Injection

Windows supports **Asynchronous Procedure Calls (APCs).**

Attackers queue malicious code into an existing thread.

The code executes when the thread becomes alertable.

---

## Thread Suspension

Threads may be suspended during:

- Debugging
- Malware analysis
- Process hollowing
- Memory injection

---

## Thread Context Manipulation

Attackers modify:

- Instruction Pointer
- Registers
- Stack

This redirects execution without creating new threads.

---

# Defensive Perspective

Security solutions monitor threads for:

- Unexpected thread creation
- Remote thread injection
- Thread hijacking
- Suspicious execution addresses
- Threads running from non-image memory

Thread behavior is an important indicator of compromise.

---

# Pentester Perspective

When assessing a Windows system, pentesters look for:

- Remote thread creation
- Injected threads
- Suspended processes
- Abnormal thread counts
- Threads executing outside legitimate modules

Many post-exploitation techniques rely on thread manipulation.

---

# Relationship with Other Components

```
Application

      │

Process (EPROCESS)

      │

Threads (ETHREAD)

      │

Windows Scheduler

      │

CPU
```

The scheduler works with threads, while the process provides the execution environment.

---

# Common Attack Techniques Involving Threads

| Technique | Description |
|-----------|-------------|
| CreateRemoteThread | Execute code in another process |
| Thread Hijacking | Redirect existing thread execution |
| APC Injection | Queue malicious code into a thread |
| Process Hollowing | Resume a manipulated thread |
| Reflective DLL Injection | Execute injected DLL via threads |

---

# Key Takeaways

- A thread is the smallest unit of execution in Windows.
- Every process contains one or more threads.
- Threads share process resources but maintain their own execution state.
- Windows schedules threads, not processes.
- Most code injection techniques ultimately rely on manipulating threads.
- Monitoring thread behavior is critical for detecting modern malware and post-exploitation activity.

---

# Key Insight

> Processes provide the environment, but threads perform the execution. In modern offensive security, controlling a thread often means controlling the behavior of an entire process.
