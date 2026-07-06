# Linux Fundamentals (Security Perspective)

## Introduction

Linux is a multi-user, multi-tasking operating system that powers the majority of modern servers, cloud platforms, containers, embedded devices, and cybersecurity infrastructure.

From a security perspective, Linux is one of the most common targets during infrastructure, web application, cloud, and internal network assessments.

Understanding Linux is essential for identifying vulnerabilities, performing privilege escalation, establishing persistence, and analyzing system behavior.

---

# Why Linux Matters in Security

Linux dominates critical infrastructure, including:

- Web Servers
- Application Servers
- Database Servers
- Cloud Platforms
- Containers
- Kubernetes Clusters
- DevOps Infrastructure
- Network Appliances

As a result, Linux systems frequently become high-value targets during penetration tests.

---

# Linux Architecture Overview

A Linux system consists of several major components:

- Kernel
- Shell
- File System
- User Space
- Services (Daemons)
- Package Management
- Networking Stack

Each component exposes its own attack surface and security considerations.

---

# Kernel

The Linux kernel manages:

- CPU scheduling
- Memory allocation
- Device drivers
- Process management
- Networking
- Security enforcement

### Security Relevance

Attackers may target:

- Kernel vulnerabilities
- Privilege escalation exploits
- Vulnerable drivers
- Kernel modules
- Rootkits

---

# Shell

The shell provides the command-line interface between users and the operating system.

Common shells include:

- Bash
- Zsh
- Fish
- Dash

### Security Relevance

Shells are commonly abused for:

- Command execution
- Reverse shells
- Persistence
- Script execution
- Privilege escalation

---

# File System

Linux stores nearly everything as files.

Important directories include:

| Directory | Purpose |
|------------|---------|
| /etc | System configuration |
| /home | User directories |
| /root | Root user home |
| /var | Logs and application data |
| /tmp | Temporary files |
| /usr | User applications |
| /bin | Essential binaries |
| /opt | Optional software |
| /dev | Device files |
| /proc | Kernel and process information |

### Security Relevance

Pentesters look for:

- Configuration files
- Password files
- SSH keys
- Backup files
- Sensitive credentials
- Writable directories

---

# Users and Permissions

Linux implements discretionary access control using:

- Users
- Groups
- Permissions
- Ownership

Permission types:

- Read (r)
- Write (w)
- Execute (x)

### Security Relevance

Common privilege escalation opportunities include:

- Weak file permissions
- Writable system files
- SUID binaries
- SGID binaries
- Sticky bit abuse
- Misconfigured sudo rules

---

# Processes

Processes represent running programs.

Useful concepts include:

- Process ID (PID)
- Parent Process (PPID)
- Threads
- Signals

### Security Relevance

Attackers inspect processes to:

- Identify running services
- Locate credentials
- Inject malicious code
- Hide malware
- Maintain persistence

---

# Services (Daemons)

Services perform background tasks without user interaction.

Examples:

- sshd
- nginx
- apache2
- mysql
- docker

Most modern Linux systems use **systemd** for service management.

### Security Relevance

Common findings include:

- Vulnerable services
- Weak service permissions
- Misconfigured startup scripts
- Insecure service accounts

---

# Package Management

Software is installed through package managers such as:

- APT
- DNF
- YUM
- Pacman

### Security Relevance

Outdated packages frequently contain publicly known vulnerabilities.

Package integrity should also be verified to prevent supply chain attacks.

---

# Networking

Linux provides native support for:

- TCP/IP
- Routing
- Firewalling
- Network interfaces
- DNS resolution

### Security Relevance

Pentesters analyze:

- Open ports
- Listening services
- Firewall rules
- Active connections
- Internal routing

---

# Logging

Linux records system activity through log files.

Common locations:

- /var/log/
- journalctl (systemd)

### Security Relevance

Logs reveal:

- Authentication attempts
- Service failures
- Command execution
- User activity

Attackers may attempt to modify or erase logs to reduce forensic evidence.

---

# Common Linux Misconfigurations

Frequently encountered issues include:

- Weak file permissions
- Writable system directories
- SUID/SGID abuse
- Weak sudo configuration
- Exposed SSH services
- Default credentials
- Unpatched software
- Misconfigured cron jobs
- Insecure NFS shares
- Weak Docker configurations

---

# Pentester Perspective

When assessing a Linux system, common questions include:

- Which users exist?
- Which services are running?
- Which ports are exposed?
- Which files contain credentials?
- Can privileges be escalated?
- Which scheduled tasks exist?
- Which binaries have elevated permissions?
- Are containers present?
- Can persistence be established?

Understanding the interaction between these components enables attackers to move from initial access to full system compromise.

---

# Key Takeaways

- Linux powers a significant portion of modern enterprise infrastructure.
- Most successful compromises result from misconfigurations, excessive privileges, or outdated software rather than weaknesses in Linux itself.
- Effective penetration testing requires understanding Linux architecture, permission models, service management, and system internals—not just command-line usage.
