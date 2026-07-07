# proc Filesystem (Security Perspective)

## Overview

The **/proc** filesystem (procfs) is a virtual filesystem provided by the Linux kernel that exposes information about running processes, kernel state, hardware, and system configuration.

Unlike traditional filesystems, **/proc does not store data on disk**. Instead, its contents are generated dynamically by the kernel whenever they are accessed.

From a security perspective:

> The `/proc` filesystem provides a live view of the operating system. It is one of the most valuable sources of information for system administrators, incident responders, and attackers performing enumeration.

Understanding `/proc` is essential for Linux internals, malware analysis, privilege escalation, system monitoring, and digital forensics.

---

# What is procfs?

`procfs` is a pseudo-filesystem mounted at:

```
/proc
```

It provides access to:

- Running processes
- CPU information
- Memory information
- Kernel parameters
- Network information
- System statistics

Everything is generated dynamically by the kernel.

---

# High-Level Architecture

```
Applications

↓

Read /proc

↓

Linux Kernel

↓

Live System Information
```

The kernel generates the requested information in real time.

---

# Why /proc Exists

The `/proc` filesystem allows user-space programs to inspect the current state of the operating system without requiring direct kernel access.

Examples include:

- `ps`
- `top`
- `htop`
- `free`
- `lsof`
- `netstat`
- `ss`

Most of these utilities obtain information from `/proc`.

---

# Process Directories

Each running process has its own directory.

Example:

```
/proc/1
/proc/250
/proc/1348
```

The directory name corresponds to the Process ID (PID).

---

# Common Process Files

### cmdline

```
/proc/<PID>/cmdline
```

Shows:

- Command-line arguments

Security relevance:

Useful for identifying how a process was started.

---

### exe

```
/proc/<PID>/exe
```

Symbolic link to the executable running the process.

Security relevance:

Allows investigators to identify the actual binary in use.

---

### cwd

```
/proc/<PID>/cwd
```

Current working directory of the process.

---

### environ

```
/proc/<PID>/environ
```

Contains environment variables.

Security relevance:

May expose:

- API keys
- Database credentials
- Tokens
- Secrets

---

### maps

```
/proc/<PID>/maps
```

Shows the process memory layout.

Includes:

- Stack
- Heap
- Shared libraries
- Memory mappings

Security relevance:

Important for exploit development and memory analysis.

---

### fd

```
/proc/<PID>/fd/
```

Contains symbolic links to all open file descriptors.

May include:

- Files
- Pipes
- Network sockets
- Devices

Security relevance:

Useful for identifying files and sockets used by a process.

---

### status

```
/proc/<PID>/status
```

Displays process metadata.

Examples:

- PID
- PPID
- UID
- GID
- Memory usage
- Thread count
- Capabilities

---

# Important System Files

## CPU Information

```
/proc/cpuinfo
```

Contains:

- CPU model
- Number of cores
- CPU flags
- Architecture

---

## Memory Information

```
/proc/meminfo
```

Shows:

- Total RAM
- Free memory
- Buffers
- Cache
- Swap usage

---

## Kernel Version

```
/proc/version
```

Displays:

- Kernel version
- Compiler
- Build information

Security relevance:

Kernel version is useful when assessing known vulnerabilities.

---

## Uptime

```
/proc/uptime
```

Shows:

- System uptime
- Idle time

---

## Mounted Filesystems

```
/proc/mounts
```

Lists all mounted filesystems.

Security relevance:

Useful for identifying:

- NFS shares
- Containers
- Mounted disks

---

## Kernel Parameters

```
/proc/sys/
```

Contains configurable kernel settings.

Examples:

```
/proc/sys/kernel/

/proc/sys/net/

/proc/sys/vm/
```

These values can often be modified using `sysctl`.

---

# Network Information

Examples:

```
/proc/net/tcp

/proc/net/udp

/proc/net/dev

/proc/net/route
```

Provides:

- Active connections
- Interfaces
- Routing table
- Network statistics

---

# Relationship with the Kernel

```
User Process

↓

Read /proc

↓

Kernel

↓

Generate Information

↓

Return Data
```

No physical files exist on disk.

---

# Security Importance

The `/proc` filesystem reveals:

- Running processes
- Open files
- Memory mappings
- Network connections
- Loaded kernel information
- Hardware configuration

It is one of the richest sources of system intelligence.

---

# Common Security Risks

Sensitive information may be exposed through:

- Environment variables
- Command-line arguments
- Open file descriptors
- Kernel information
- Memory mappings

Improper permissions can allow information disclosure.

---

# Offensive Perspective

Attackers frequently inspect `/proc` to identify:

- Running services
- Privileged processes
- Open sockets
- Credentials in environment variables
- Kernel version
- Loaded libraries
- Container environments
- Memory layouts

This information helps build attack paths and identify privilege escalation opportunities.

---

# Defensive Perspective

Security teams use `/proc` to:

- Investigate suspicious processes
- Monitor system resources
- Detect unauthorized services
- Analyze memory usage
- Verify running binaries
- Perform incident response

Many monitoring tools rely directly on `/proc`.

---

# Common proc Files

| Path | Purpose |
|------|---------|
| `/proc/cpuinfo` | CPU information |
| `/proc/meminfo` | Memory statistics |
| `/proc/version` | Kernel version |
| `/proc/uptime` | System uptime |
| `/proc/mounts` | Mounted filesystems |
| `/proc/net/` | Network information |
| `/proc/sys/` | Kernel parameters |

---

# Common Process Files

| Path | Purpose |
|------|---------|
| `/proc/<PID>/cmdline` | Command-line arguments |
| `/proc/<PID>/exe` | Executable path |
| `/proc/<PID>/cwd` | Current directory |
| `/proc/<PID>/fd/` | Open file descriptors |
| `/proc/<PID>/maps` | Memory mappings |
| `/proc/<PID>/status` | Process information |
| `/proc/<PID>/environ` | Environment variables |

---

# Why /proc Matters

Understanding `/proc` helps security professionals:

- Enumerate systems
- Analyze running processes
- Investigate malware
- Inspect memory layouts
- Perform digital forensics
- Assess privilege escalation opportunities

---

# Key Takeaways

- `/proc` is a virtual filesystem generated dynamically by the Linux kernel.
- Every running process has its own directory under `/proc/<PID>`.
- Process directories expose information about memory, files, execution, and credentials.
- System-wide files provide information about hardware, memory, networking, and kernel configuration.
- Both attackers and defenders rely heavily on `/proc` for system enumeration and analysis.

---

# Key Insight

> The `/proc` filesystem is a live interface to the Linux kernel. It exposes the operating system's current state in real time, making it one of the most powerful sources of information for both system administrators and attackers.
