# Control Groups (cgroups) (Security Perspective)

## Overview

**Control Groups (cgroups)** are a Linux kernel feature that limits, monitors, and isolates the resource usage of groups of processes.

While **Namespaces** isolate what a process can *see*, **cgroups** control how many system resources a process can *consume*.

From a security perspective:

> cgroups prevent resource abuse by enforcing limits on CPU, memory, disk I/O, and other system resources, making them essential for container security and system stability.

Understanding cgroups is fundamental for Linux internals, container security, cloud computing, and denial-of-service (DoS) mitigation.

---

# What are cgroups?

A cgroup is a kernel mechanism used to organize processes into groups and apply resource policies.

Instead of managing each process individually:

```
Processes

↓

Control Group

↓

Resource Limits
```

Every process belongs to one or more cgroups.

---

# Why cgroups Exist

Without cgroups:

```
All Processes

↓

Compete Freely

↓

Resource Starvation
```

With cgroups:

```
Process Group

↓

Defined Limits

↓

Fair Resource Allocation
```

This prevents a single application from exhausting system resources.

---

# High-Level Architecture

```
Linux Kernel

↓

Control Groups

├── Group A

│      ├── Process 1

│      └── Process 2

│

├── Group B

│      ├── Process 3

│      └── Process 4

│

└── Group C
```

Each group has independent resource constraints.

---

# What Can cgroups Control?

cgroups can manage:

- CPU usage
- Memory usage
- Disk I/O
- Network priorities
- Process count
- Device access
- Huge pages
- Swap usage

---

# CPU Control

CPU resources can be limited or prioritized.

Example:

```
Web Server

↓

40% CPU

Database

↓

60% CPU
```

This prevents CPU starvation.

---

# Memory Control

Limits:

- Maximum RAM
- Swap usage
- Memory pressure

Example:

```
Application

↓

512 MB RAM

↓

Exceeded

↓

OOM Killer
```

---

# Process Limits

Limits the number of processes.

Example:

```
Container

↓

Maximum 200 Processes
```

Prevents:

- Fork bombs
- Process exhaustion

---

# Disk I/O Control

Limits:

- Read speed
- Write speed
- Disk priority

Useful for:

- Shared servers
- Multi-tenant environments

---

# Device Access

Controls access to devices.

Examples:

- USB devices
- Block devices
- Character devices

Prevents unauthorized hardware access.

---

# cgroup Hierarchy

Control groups form a hierarchy.

```
Root cgroup

│

├── system.slice

├── user.slice

└── docker.slice
```

Each child inherits restrictions from its parent.

---

# cgroup v1 vs cgroup v2

Linux currently supports:

| Version | Description |
|----------|-------------|
| cgroup v1 | Multiple independent hierarchies |
| cgroup v2 | Unified hierarchy and improved design |

Modern Linux distributions increasingly use **cgroup v2**.

---

# Relationship with systemd

Modern Linux systems integrate cgroups with `systemd`.

```
systemd

↓

Creates cgroups

↓

Manages Services

↓

Enforces Limits
```

Each service typically runs within its own control group.

---

# Relationship with Containers

Containers combine:

```
Namespaces

+

cgroups

+

Capabilities

+

Filesystem Isolation
```

Namespaces provide isolation.

cgroups provide resource control.

---

# Example

```
Docker Container

↓

2 CPUs

↓

1 GB RAM

↓

100 Processes

↓

Disk I/O Limit
```

The kernel enforces these limits regardless of application behavior.

---

# Security Importance

Without cgroups:

A malicious application could:

- Consume all memory
- Use all CPU cores
- Spawn unlimited processes
- Cause denial of service

cgroups prevent these scenarios by enforcing strict resource boundaries.

---

# Common Security Risks

Misconfigured cgroups may allow:

- Resource exhaustion
- Container escape assistance
- Service instability
- Denial-of-Service attacks

Improper limits reduce system resilience.

---

# Offensive Perspective

Attackers analyze cgroups to determine:

- Resource restrictions
- Container environment
- Available CPU
- Memory limits
- Process limits

They may attempt to:

- Exhaust allocated resources
- Trigger Out-Of-Memory conditions
- Abuse weak resource policies

---

# Defensive Perspective

Administrators should:

- Apply CPU quotas
- Set memory limits
- Restrict process counts
- Limit device access
- Monitor resource usage
- Use cgroup v2 when possible

Proper cgroup configuration improves both security and availability.

---

# Common Attack Scenarios

| Technique | Goal |
|-----------|------|
| Fork Bomb | Exhaust process table |
| Memory Exhaustion | Crash services |
| CPU Exhaustion | Denial-of-Service |
| Disk I/O Abuse | Slow down system |
| Resource Starvation | Affect neighboring services |

---

# cgroups vs Namespaces

| Namespaces | cgroups |
|------------|----------|
| Resource isolation | Resource limitation |
| Hide resources | Limit resources |
| Process visibility | Resource consumption |
| Separate environments | Control usage |
| Security boundary | Resource enforcement |

Together, they provide the foundation for Linux containers.

---

# Why cgroups Matter

Understanding cgroups helps security professionals:

- Secure container platforms
- Prevent resource abuse
- Investigate DoS attacks
- Harden Linux servers
- Analyze cloud workloads
- Understand Kubernetes resource management

---

# Key Takeaways

- cgroups are a Linux kernel feature for controlling resource usage.
- They manage CPU, memory, disk I/O, processes, and device access.
- Every process belongs to one or more control groups.
- Modern Linux systems integrate cgroups with `systemd`.
- Containers rely on cgroups to enforce resource isolation.
- Proper cgroup configuration is essential for preventing resource exhaustion and maintaining system stability.

---

# Key Insight

> Namespaces determine **what a process can see**, while cgroups determine **how much it can use**. Together, they form the foundation of modern Linux containerization, balancing isolation with controlled resource allocation.
