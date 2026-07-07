# systemd and Services (Security Perspective)

## Overview

`systemd` is the default init system and service manager used by most modern Linux distributions. It is the first userspace process started by the kernel and is responsible for initializing the system, managing services, and controlling the system lifecycle.

From a security perspective:

> systemd controls what starts, when it starts, and under which privileges it executes. Misconfigured or compromised services can become entry points for privilege escalation, persistence, and lateral movement.

Understanding `systemd` is essential for Linux internals, incident response, system hardening, and offensive security.

---

# What is systemd?

`systemd` is the first userspace process launched after the kernel has finished initialization.

It always runs as:

```
PID 1
```

Responsibilities include:

- Starting services
- Managing dependencies
- Handling logging
- Mounting filesystems
- Managing user sessions
- Controlling shutdown and reboot
- Managing system targets

---

# High-Level Architecture

```
Linux Kernel

↓

systemd (PID 1)

↓

System Targets

↓

Services

↓

Applications
```

---

# Why systemd Exists

Older Linux systems used the SysV init system, which had limited dependency management and slower boot times.

systemd improves this by providing:

- Parallel service startup
- Dependency resolution
- Service monitoring
- Automatic restart
- Unified management interface

---

# Units

Everything managed by systemd is represented as a **Unit**.

Common unit types include:

| Unit | Purpose |
|------|---------|
| `.service` | Background services |
| `.socket` | Socket activation |
| `.mount` | Filesystem mounts |
| `.target` | System state/group |
| `.timer` | Scheduled tasks |
| `.device` | Hardware devices |
| `.path` | File monitoring |

---

# Service Units

A service is defined using a `.service` file.

Example:

```
sshd.service

nginx.service

docker.service
```

Service unit files describe:

- Executable
- User
- Dependencies
- Restart policy
- Environment variables

---

# Unit File Structure

A typical service file contains:

```
[Unit]

Description=

After=

Requires=

[Service]

ExecStart=

User=

Group=

Restart=

[Install]

WantedBy=
```

These settings determine how and when a service executes.

---

# Targets

Targets define system states.

Examples:

| Target | Purpose |
|---------|---------|
| `multi-user.target` | Multi-user CLI |
| `graphical.target` | Graphical desktop |
| `rescue.target` | Recovery mode |
| `emergency.target` | Minimal environment |

Targets replace the traditional Linux runlevels.

---

# Service Lifecycle

```
Disabled

↓

Enabled

↓

Started

↓

Running

↓

Stopped

↓

Restarted
```

systemd continuously monitors managed services.

---

# Dependency Management

Services may depend on other services.

Example:

```
Network

↓

Database

↓

Web Server

↓

Application
```

systemd ensures dependencies are started in the correct order.

---

# Journald

systemd includes its own logging service:

```
systemd-journald
```

Responsibilities:

- Collect system logs
- Store service logs
- Capture kernel messages
- Record boot events

Security relevance:

Useful for:

- Incident response
- Service troubleshooting
- Security investigations

---

# Socket Activation

systemd can delay starting a service until it is actually needed.

Example:

```
Incoming Connection

↓

Socket Unit

↓

Start Service
```

Benefits:

- Faster boot
- Lower resource usage

---

# Security Importance

Services often run with elevated privileges.

Common examples:

- sshd
- nginx
- docker
- cron
- rsyslog

Compromising one of these services may provide:

- Initial access
- Privilege escalation
- Persistence

---

# Common Security Risks

Misconfigured services may include:

- Running as root unnecessarily
- Writable service binaries
- Weak file permissions
- Insecure environment variables
- Automatic restart of malicious binaries
- Insecure service dependencies

---

# Privileged Services

Many services execute as:

```
root
```

Examples:

- sshd
- systemd
- NetworkManager
- systemd-logind

If exploited, these services may provide full system compromise.

---

# Service Files

Service definitions are commonly stored in:

```
/etc/systemd/system/

/usr/lib/systemd/system/

/lib/systemd/system/
```

Security relevance:

Attackers may attempt to modify service files to establish persistence.

---

# Relationship with PID 1

```
Kernel

↓

systemd (PID 1)

↓

Service Manager

↓

All Services
```

Every long-running service is ultimately managed by PID 1.

---

# Offensive Perspective

Attackers inspect systemd to identify:

- Running services
- Startup services
- Privileged services
- Writable service files
- Misconfigured unit files
- Automatic restart behavior

Common goals include:

- Privilege escalation
- Persistence
- Code execution

---

# Defensive Perspective

Administrators should:

- Minimize privileged services
- Disable unused services
- Restrict service permissions
- Protect service unit files
- Audit startup configuration
- Monitor unexpected services
- Review service logs regularly

---

# Common Attack Scenarios

| Technique | Target |
|-----------|--------|
| Writable service file | Persistence |
| Writable executable | Privilege escalation |
| Weak service permissions | Code execution |
| Malicious systemd service | Persistence |
| Service hijacking | Privilege escalation |

---

# Common Directories

| Path | Purpose |
|------|---------|
| `/etc/systemd/system/` | Local service configuration |
| `/usr/lib/systemd/system/` | Distribution service files |
| `/run/systemd/` | Runtime information |
| `/var/log/journal/` | Journal logs (if persistent) |

---

# Why systemd Matters

Understanding systemd helps security professionals:

- Analyze Linux boot behavior
- Investigate persistence
- Audit services
- Detect privilege escalation paths
- Perform incident response
- Harden Linux systems

---

# Key Takeaways

- `systemd` is the default init system for most modern Linux distributions.
- It always runs as PID 1 and manages the system lifecycle.
- Everything managed by systemd is represented as a unit.
- Services are configured using `.service` unit files.
- Many privileged services execute under root, making them attractive attack targets.
- Misconfigured services are a common source of privilege escalation and persistence.

---

# Key Insight

> systemd is more than a service manager it is the central orchestrator of the Linux operating system. Controlling systemd or its managed services often means controlling how the entire system starts, runs, and maintains trust.
