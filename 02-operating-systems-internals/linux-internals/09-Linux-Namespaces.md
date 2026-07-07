# Linux Namespaces (Security Perspective)

## Overview

Linux **Namespaces** are a kernel feature that isolates system resources so that processes see their own independent view of the operating system.

Namespaces are one of the core technologies behind **containers** (Docker, Kubernetes, Podman, LXC).

From a security perspective:

> Namespaces isolate processes from each other and from the host, reducing the impact of compromise and providing the foundation for container security.

Understanding namespaces is essential for Linux internals, container security, privilege escalation, and cloud security.

---

# What are Namespaces?

Normally, every process shares the same system resources:

- Process IDs
- Network interfaces
- Hostname
- Mount points
- IPC objects

Namespaces create isolated instances of these resources.

```
Host

↓

Namespaces

↓

Isolated Process View
```

Each namespace provides its own independent environment.

---

# Why Namespaces Exist

Without namespaces:

```
All Processes

↓

Same System Resources
```

With namespaces:

```
Namespace A

↓

Own Resources

Namespace B

↓

Own Resources
```

This isolation allows multiple environments to coexist securely on the same kernel.

---

# High-Level Architecture

```
Linux Kernel

│

├── Namespace 1

│     ├── Process A

│     └── Process B

│

├── Namespace 2

│     ├── Process C

│     └── Process D

│

└── Namespace 3

      └── Process E
```

All namespaces share the same kernel while maintaining isolated resources.

---

# Types of Namespaces

Linux supports several namespace types.

| Namespace | Isolates |
|------------|----------|
| PID | Process IDs |
| Mount | Filesystem mounts |
| Network | Network stack |
| IPC | Inter-Process Communication |
| UTS | Hostname & domain name |
| User | User & group IDs |
| Cgroup | Resource control |
| Time | System clocks |

---

# PID Namespace

Isolates:

- Process IDs

Example:

Container:

```
PID 1

↓

Application
```

Host:

```
PID 25438

↓

Same Application
```

The process believes it is PID 1 inside the container.

---

# Mount Namespace

Provides isolated filesystem views.

Example:

```
Container A

↓

/data

Container B

↓

Different /data
```

Processes cannot see each other's mount points.

---

# Network Namespace

Provides an isolated networking stack.

Each namespace has its own:

- Interfaces
- Routing table
- Firewall rules
- ARP table
- Ports

Example:

```
Container A

↓

eth0

Container B

↓

Independent eth0
```

---

# UTS Namespace

Isolates:

- Hostname
- Domain name

Each container may have its own hostname.

---

# IPC Namespace

Isolates:

- Shared memory
- Message queues
- Semaphores

Processes cannot access IPC objects outside their namespace.

---

# User Namespace

Maps user IDs inside a namespace.

Example:

```
Container

root (UID 0)

↓

Host

UID 100000
```

This allows a process to appear as root inside the container without being root on the host.

---

# Cgroup Namespace

Provides isolated views of resource control groups.

Used with:

- CPU limits
- Memory limits
- Disk limits

Common in container environments.

---

# Time Namespace

Allows independent system clocks.

Useful for:

- Testing
- Containers
- Virtualized environments

---

# Relationship with Containers

Containers are primarily built using:

```
Namespaces

+

Control Groups (cgroups)

+

Capabilities

+

Filesystem Isolation
```

Namespaces provide isolation, while cgroups enforce resource limits.

---

# Shared Kernel

Although namespaces isolate resources, all containers share the same Linux kernel.

```
Container A

↓

Shared Kernel

↑

Container B
```

This is why kernel vulnerabilities can affect multiple containers.

---

# Security Importance

Namespaces isolate:

- Processes
- Filesystems
- Networking
- Hostnames
- IPC
- Users

This reduces the blast radius of a compromised process.

---

# Common Security Risks

Misconfigured namespaces may allow:

- Container escape
- Namespace escape
- Host interaction
- Privilege escalation
- Information disclosure

Isolation is only as strong as its configuration.

---

# Offensive Perspective

Attackers analyze namespaces to determine:

- Whether they are inside a container
- Available capabilities
- Namespace boundaries
- User mappings
- Shared resources
- Escape opportunities

A compromised container is often the first step toward attacking the host.

---

# Defensive Perspective

Administrators should:

- Enable user namespaces
- Minimize container privileges
- Combine namespaces with cgroups
- Remove unnecessary capabilities
- Keep the kernel updated
- Restrict host resource sharing

Namespaces should be part of a layered security model.

---

# Namespaces vs Virtual Machines

| Namespace (Containers) | Virtual Machine |
|-------------------------|-----------------|
| Shares host kernel | Own kernel |
| Lightweight | Heavier |
| Fast startup | Slower startup |
| Lower overhead | Higher overhead |
| Isolation at OS level | Isolation at hardware level |

---

# Common Attack Scenarios

| Technique | Goal |
|-----------|------|
| Container Escape | Access host system |
| Namespace Escape | Break isolation |
| User Namespace Abuse | Privilege escalation |
| Shared Kernel Exploit | Host compromise |
| Capability Abuse | Escape restrictions |

---

# Why Namespaces Matter

Understanding namespaces helps security professionals:

- Analyze container security
- Investigate container escapes
- Harden Linux systems
- Secure Kubernetes environments
- Understand modern cloud infrastructure
- Assess isolation boundaries

---

# Key Takeaways

- Linux Namespaces isolate system resources for groups of processes.
- Each namespace provides an independent view of resources such as processes, networking, and filesystems.
- Containers rely heavily on namespaces for isolation.
- All namespaces share the same Linux kernel, making kernel security critical.
- Namespaces are most effective when combined with cgroups, capabilities, and least-privilege principles.
- Misconfigured namespaces can lead to container escape and privilege escalation.

---

# Key Insight

> Namespaces are the foundation of Linux container isolation. They do not create separate operating systems—they create separate views of the same kernel. Security depends not only on isolation, but also on how well that isolation is configured and enforced.
