# Processes and Threads (Security Perspective)

## Overview

A **process** is an instance of a running program, while a **thread** is the smallest unit of execution within that process.

From a security perspective:

> Every program executes as a process, but the operating system schedules and executes individual threads. Understanding both is fundamental for process injection, privilege escalation, malware analysis, and memory forensics.

---

# High-Level Architecture

```
Program (Executable)

↓

Process

├── Virtual Memory
├── Open Files
├── File Descriptors
├── Environment Variables
└── Threads

        │
        ├── Thread 1
        ├── Thread 2
        └── Thread N
```

---

# What is a Process?

A process is an isolated execution environment created by the kernel.

A process contains:

- Process ID (PID)
- Virtual memory
- File descriptors
- Environment variables
- Credentials
- One or more threads

Each running application has its own process.

Examples:

- bash
- ssh
- nginx
- firefox
- python

---

# Process Lifecycle

```
Program

↓

fork()

↓

Child Process

↓

execve()

↓

Running

↓

Exit

↓

Zombie (temporary)

↓

Removed
```

---

# Process Components

| Component | Purpose |
|----------|---------|
| PID | Unique process identifier |
| Virtual Memory | Private address space |
| Threads | Units of execution |
| File Descriptors | Open resources |
| Credentials | User and group IDs |
| Environment Variables | Runtime configuration |

---

# Process IDs (PID)

Every process receives a unique Process ID.

Example:

```
PID 1

↓

systemd
```

Examples:

```
PID 324

bash

PID 918

sshd

PID 1500

nginx
```

---

# Parent and Child Processes

Linux processes form a hierarchy.

```
systemd (PID 1)

↓

sshd

↓

bash

↓

python
```

Every process (except PID 1) has a parent.

---

# Process Creation

Linux creates processes using:

```
fork()
```

The child process is almost identical to the parent.

To execute another program:

```
execve()
```

is called.

---

# What is a Thread?

A thread is the smallest execution unit managed by the scheduler.

A process contains at least one thread.

Multiple threads execute within the same process.

---

# Thread Characteristics

Threads share:

- Virtual memory
- Heap
- Global variables
- Open files

Threads have their own:

- Stack
- Registers
- Program Counter
- Thread ID (TID)

---

# Process vs Thread

| Process | Thread |
|----------|---------|
| Independent execution unit | Execution unit inside a process |
| Own virtual memory | Shared process memory |
| Own file descriptor table | Shared with process |
| Higher creation cost | Lightweight |
| Better isolation | Faster communication |

---

# Process Memory Layout

```
+----------------------+
| Stack                |
+----------------------+
| Heap                 |
+----------------------+
| Shared Libraries     |
+----------------------+
| Data                 |
+----------------------+
| Code (.text)         |
+----------------------+
```

All threads inside the process share this memory except for their own stacks.

---

# Context Switching

The scheduler switches CPU execution between threads.

```
Thread A

↓

Scheduler

↓

Thread B

↓

Scheduler

↓

Thread C
```

Rapid switching creates the illusion of parallel execution.

---

# Linux Scheduler

The scheduler decides:

- Which thread runs
- CPU allocation
- Priority
- Fairness

Security relevance:

CPU starvation or abuse can lead to Denial-of-Service (DoS).

---

# File Descriptors

Each process owns file descriptors.

Examples:

| Descriptor | Purpose |
|------------|----------|
| 0 | Standard Input |
| 1 | Standard Output |
| 2 | Standard Error |

Additional descriptors represent:

- Files
- Pipes
- Network sockets
- Devices

---

# Signals

Processes communicate using signals.

Examples:

| Signal | Purpose |
|---------|----------|
| SIGTERM | Graceful termination |
| SIGKILL | Immediate termination |
| SIGSTOP | Pause process |
| SIGCONT | Resume process |

---

# Process States

Common states include:

| State | Description |
|--------|-------------|
| Running | Currently executing |
| Sleeping | Waiting for event |
| Stopped | Execution paused |
| Zombie | Finished but not cleaned up |
| Orphan | Parent process terminated |

---

# Security Relevance

Processes define execution boundaries.

Attackers analyze processes to:

- Identify running services
- Locate privileged processes
- Discover credentials
- Inject malicious code
- Escalate privileges

---

# Common Attack Techniques

| Technique | Target |
|-----------|--------|
| Process Injection | Running process |
| Process Hollowing | Legitimate process |
| DLL/Library Injection | Process memory |
| Memory Dumping | Process memory |
| Thread Hijacking | Existing thread |
| Code Injection | Running process |

---

# Offensive Perspective

During post-exploitation, attackers enumerate:

- Running processes
- Parent-child relationships
- High-privilege services
- Long-running daemons
- Security software
- Network services

Processes often reveal valuable attack opportunities.

---

# Defensive Perspective

Security teams monitor:

- Suspicious processes
- Unexpected parent-child relationships
- High CPU usage
- Memory anomalies
- Unauthorized process creation
- Process injection attempts

Endpoint Detection and Response (EDR) solutions rely heavily on process monitoring.

---

# Process Hierarchy Example

```
systemd

├── sshd

│      └── bash

│              └── python

├── nginx

├── docker

└── cron
```

---

# Key Takeaways

- A process is an isolated execution environment created by the Linux kernel.
- Every process contains one or more threads.
- Threads share process resources while maintaining independent execution contexts.
- The scheduler manages thread execution across CPUs.
- Processes provide isolation, while threads provide concurrency.
- Understanding processes and threads is fundamental for privilege escalation, malware analysis, process injection, and incident response.

---

# Key Insight

> Processes define the boundaries of execution, while threads perform the actual work. In Linux security, controlling a privileged process—or one of its threads—often means controlling the actions that process can perform on behalf of the operating system.
