# Processes and Memory (Security Perspective)

## Introduction

Processes and memory are fundamental components of every operating system.

A process represents a running program, while memory provides the workspace required for execution.

From a security perspective, processes and memory form the primary execution environment where applications run, privileges are enforced, and attacks are executed.

> Most exploitation techniques ultimately aim to manipulate processes, memory, or both.

---

# Why Processes and Memory Matter

Every application relies on processes and memory to:

- Execute instructions
- Store data
- Communicate with the operating system
- Access files and network resources
- Maintain user sessions

Compromising a privileged process often leads to complete system compromise.

---

# Processes

A process is an active instance of a program.

Each process contains:

- Executable code
- Process ID (PID)
- Memory space
- Threads
- Open files
- Network connections
- Security context

Every running application creates one or more processes.

---

# Threads

A thread is the smallest unit of execution within a process.

A process may contain multiple threads executing simultaneously.

### Security Relevance

Attackers may abuse threads for:

- Code injection
- Remote thread creation
- Process manipulation
- Malware execution

---

# Process Lifecycle

Processes typically follow these stages:

- Creation
- Execution
- Waiting
- Termination

Attackers often attempt to inject malicious code during the execution phase.

---

# Process Management

## Linux

Common utilities include:

- ps
- top
- htop
- kill
- systemctl

---

## Windows

Common tools include:

- Task Manager
- Process Explorer
- services.exe
- tasklist
- taskkill

High-value processes include:

- lsass.exe
- winlogon.exe
- explorer.exe
- svchost.exe

---

# Memory

Memory stores everything required during program execution.

It contains:

- Executable code
- Variables
- Libraries
- User data
- Session information
- Cryptographic material
- Credentials

Memory is temporary and normally cleared when the process terminates.

---

# Virtual Memory

Modern operating systems implement virtual memory.

Each process receives its own isolated address space.

### Security Benefits

- Process isolation
- Memory protection
- Controlled resource allocation

Improper isolation may lead to information disclosure or privilege escalation.

---

# Memory Layout

A typical process memory layout contains:

- Code Segment (.text)
- Data Segment
- Heap
- Stack
- Shared Libraries

Each region serves a different purpose during execution.

---

# User Mode vs Kernel Mode

## User Mode

Applications execute with limited privileges.

Access to hardware is restricted.

---

## Kernel Mode

The operating system kernel executes with full privileges.

Compromising kernel mode usually results in complete system compromise.

---

# Why Memory Matters in Security

Sensitive information frequently resides in memory, including:

- User credentials
- Authentication tokens
- Encryption keys
- Session cookies
- API secrets

Attackers often target memory instead of disk because valuable information may never be written to storage.

---

# Common Attack Techniques

## Process Injection

Injecting malicious code into a legitimate process.

Purpose:

- Defense evasion
- Payload execution
- Privilege abuse

---

## Process Hollowing

Creating a legitimate process and replacing its memory with malicious code.

Commonly used by malware.

---

## DLL Injection

Loading malicious dynamic libraries into trusted processes.

Often used for persistence and evasion.

---

## Credential Dumping

Extracting credentials stored in memory.

Common targets include:

- LSASS
- Kerberos tickets
- NTLM credentials

---

## Buffer Overflow

Writing beyond allocated memory boundaries.

Potential impact:

- Arbitrary code execution
- Application crashes
- Privilege escalation

---

## Use-After-Free

Abusing freed memory that is still referenced.

Frequently exploited in modern vulnerability research.

---

# Memory Protection Mechanisms

Operating systems implement multiple protections, including:

- DEP (Data Execution Prevention)
- ASLR (Address Space Layout Randomization)
- Stack Canaries
- Control Flow Guard (CFG)

These mechanisms increase exploitation difficulty but do not eliminate vulnerabilities.

---

# Why Processes Matter to Pentesters

During an assessment, penetration testers analyze:

- Running processes
- High-privilege services
- Security software
- Memory-resident credentials
- Parent-child process relationships
- Suspicious process behavior

Process enumeration often reveals privilege escalation and persistence opportunities.

---

# Attacker Perspective

Attackers attempt to answer questions such as:

- Which processes run with elevated privileges?
- Which services can be abused?
- Can code be injected?
- Are credentials present in memory?
- Can trusted processes be impersonated?

Controlling a privileged process frequently provides direct control over the system.

---

# Key Takeaways

- Processes are the execution units of the operating system.
- Memory stores code, credentials, and runtime data.
- Most exploitation techniques manipulate processes or memory.
- High-privilege processes are valuable targets for privilege escalation.
- Understanding process and memory behavior is essential for offensive security.
