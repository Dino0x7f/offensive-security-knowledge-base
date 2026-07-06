# Permission Models (Security Perspective)

## Introduction

Permission models define how operating systems control access to files, directories, processes, devices, and other system resources.

From a security perspective, permission models establish the boundaries between users, applications, and privileged system components.

> Every privilege escalation attack begins with a weakness in the permission model.

---

# Why Permission Models Matter

Permission models determine:

- Who can access a resource
- What actions can be performed
- Which processes inherit privileges
- How administrative access is controlled

A single misconfigured permission can expose an entire system.

---

# Core Permission Principles

Modern operating systems implement permission models based on several security principles:

- Least Privilege
- Separation of Duties
- Need-to-Know
- Default Deny
- Access Control

These principles reduce the impact of both accidental misuse and malicious activity.

---

# Linux Permission Model

Linux primarily uses the traditional UNIX permission model.

## Standard Permissions

Each file and directory contains three permission sets:

- Owner
- Group
- Others

Each set supports:

- Read (r)
- Write (w)
- Execute (x)

---

## Ownership

Every object has:

- User Owner
- Group Owner

Ownership determines who can modify permissions and access resources.

---

## Advanced Permission Mechanisms

Linux includes several advanced permission features:

### SUID (Set User ID)

Executable runs with the permissions of its owner.

Security relevance:

- Common privilege escalation vector
- Frequently abused during post-exploitation

---

### SGID (Set Group ID)

Executable or directory inherits group ownership.

Security relevance:

- Group privilege abuse
- Shared resource management

---

### Sticky Bit

Commonly used on shared directories.

Security relevance:

- Prevents users from deleting files owned by others.

---

## Linux Security Risks

Common permission-related weaknesses include:

- Misconfigured sudo rules
- World-writable files
- Writable system directories
- Dangerous SUID binaries
- Weak ownership settings
- Writable cron jobs
- Misconfigured service permissions

---

# Windows Permission Model

Windows primarily uses Access Control Lists (ACLs).

Each object contains a list defining which users or groups can perform specific actions.

---

## NTFS Permissions

Common permissions include:

- Full Control
- Modify
- Read & Execute
- Read
- Write

Permissions can be inherited from parent directories.

---

## Security Identifiers (SIDs)

Windows identifies users and groups using unique Security Identifiers (SIDs).

Permissions are assigned to SIDs rather than usernames.

---

## Access Tokens

After authentication, Windows creates an access token containing:

- User SID
- Group memberships
- Privileges
- Security attributes

Every running process inherits the security context of its access token.

---

## User Account Control (UAC)

UAC separates administrative privileges from standard user execution.

Security relevance:

- Limits accidental privilege abuse
- Creates an additional barrier for privilege escalation

---

## Windows Security Risks

Common permission weaknesses include:

- Weak NTFS ACLs
- Writable service binaries
- Unquoted service paths
- Excessive administrative privileges
- Misconfigured scheduled tasks
- Weak registry permissions
- DLL search order hijacking

---

# Permission Inheritance

Many operating systems support permission inheritance.

Examples include:

- NTFS inherited ACLs
- Linux default ACLs
- Shared directory inheritance

Poor inheritance frequently results in unintended privilege exposure.

---

# Why Permission Models Matter to Pentesters

Permission enumeration is a fundamental step during post-exploitation.

Pentesters search for:

- Writable directories
- Writable executables
- Weak ACLs
- Dangerous SUID binaries
- Excessive privileges
- Service misconfigurations
- Registry permission issues
- Scheduled task permissions

These weaknesses often provide the shortest path to privilege escalation.

---

# Common Privilege Escalation Opportunities

Examples include:

## Linux

- SUID abuse
- sudo misconfiguration
- Writable cron jobs
- Weak file permissions
- PATH hijacking

---

## Windows

- Weak service permissions
- Unquoted service paths
- Writable registry keys
- DLL hijacking
- Weak scheduled task permissions
- Overly permissive ACLs

---

# Attacker Perspective

Attackers analyze permission models to answer questions such as:

- Can I execute privileged code?
- Can I modify trusted files?
- Can I replace a service executable?
- Can I inherit elevated privileges?
- Can I abuse another user's permissions?

Permission enumeration is one of the first activities after obtaining initial access.

---

# Key Takeaways

- Permission models define the security boundaries of an operating system.
- Least privilege significantly reduces attack opportunities.
- Misconfigured permissions remain one of the most common privilege escalation vectors.
- Effective permission enumeration is essential for both penetration testing and defensive security.
