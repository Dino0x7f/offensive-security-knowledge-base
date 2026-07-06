# Windows Fundamentals (Security Perspective)

## Introduction

Microsoft Windows is the most widely deployed operating system in enterprise environments and forms the foundation of many corporate infrastructures.

From a security perspective, Windows is a primary target for attackers due to its integration with enterprise authentication systems, centralized management, and extensive attack surface.

Understanding Windows fundamentals is essential for penetration testing, privilege escalation, Active Directory assessments, malware analysis, and incident response.

---

# Why Windows Matters in Security

Windows is commonly used for:

- Corporate Workstations
- File Servers
- Application Servers
- Domain Controllers
- Virtual Desktop Infrastructure (VDI)
- Enterprise Authentication
- Endpoint Management

Because Windows systems often store user credentials and communicate with Active Directory, compromising a single machine can become the first step toward compromising an entire enterprise.

---

# Windows Architecture Overview

A Windows system consists of several major components:

- Windows Kernel
- User Mode
- Registry
- NTFS File System
- Services
- Processes
- Security Accounts Manager (SAM)
- Local Security Authority (LSA)
- Networking Stack

Each component contributes to the system's security model and presents unique attack surfaces.

---

# Windows Kernel

The Windows kernel manages:

- Memory
- Processes
- Threads
- Device Drivers
- Hardware Communication
- Security Enforcement

### Security Relevance

Attackers may target:

- Kernel vulnerabilities
- Vulnerable drivers
- Privilege escalation exploits
- Rootkits

---

# Windows Registry

The Registry is a hierarchical database containing operating system and application configuration.

### Security Relevance

Common attacker objectives include:

- Persistence
- Configuration manipulation
- Credential discovery
- Malware execution
- Security control bypass

---

# NTFS File System

Windows primarily uses the NTFS file system.

Key security features include:

- Access Control Lists (ACLs)
- File Ownership
- Permissions
- Alternate Data Streams (ADS)
- Encryption (EFS)

### Security Relevance

Pentesters examine:

- Weak ACLs
- Writable directories
- Sensitive files
- Hidden data
- Credential storage

---

# Users and Security Principals

Windows manages access through:

- Local Users
- Domain Users
- Security Groups
- Service Accounts
- Computer Accounts

### Security Relevance

Common attack paths include:

- Weak permissions
- Privilege escalation
- Token abuse
- Group membership abuse

---

# Authentication

Windows supports several authentication mechanisms, including:

- NTLM
- Kerberos
- Local Authentication
- Active Directory Authentication

### Security Relevance

Authentication weaknesses frequently lead to:

- Credential theft
- Pass-the-Hash
- Pass-the-Ticket
- Kerberoasting
- NTLM Relay

---

# Processes and Services

Processes execute applications, while services provide background functionality.

Examples include:

- lsass.exe
- svchost.exe
- services.exe
- winlogon.exe
- explorer.exe

### Security Relevance

Attackers often target:

- Credential dumping
- Process injection
- DLL hijacking
- Service misconfigurations
- Unquoted service paths

---

# Windows Services

Services run automatically to support operating system functionality.

### Security Relevance

Common findings include:

- Weak service permissions
- Vulnerable services
- Misconfigured service accounts
- Writable service binaries

---

# Networking

Windows includes built-in networking services such as:

- SMB
- RDP
- DNS
- WinRM
- RPC

### Security Relevance

Pentesters analyze:

- Exposed services
- Network shares
- Firewall configuration
- Remote management interfaces
- Lateral movement opportunities

---

# Active Directory Integration

Most enterprise Windows systems are joined to Active Directory domains.

### Security Relevance

This introduces attack vectors such as:

- Kerberoasting
- AS-REP Roasting
- Pass-the-Hash
- Pass-the-Ticket
- DCSync
- ACL Abuse
- Delegation Abuse

---

# Event Logging

Windows records security and system events through Event Logs.

Examples include:

- Security Logs
- System Logs
- Application Logs
- PowerShell Logs

### Security Relevance

Logs reveal:

- Authentication activity
- Process creation
- Privilege changes
- Network connections

Attackers frequently attempt to disable or clear logs to reduce forensic evidence.

---

# Common Windows Misconfigurations

Frequently encountered weaknesses include:

- Weak local administrator passwords
- Unpatched operating systems
- Writable service binaries
- Weak NTFS permissions
- Unquoted service paths
- Insecure registry permissions
- Credential reuse
- Misconfigured Group Policy
- Disabled security controls
- Weak PowerShell restrictions

---

# Pentester Perspective

During a Windows assessment, penetration testers seek to determine:

- Which users and groups exist?
- Which services are exposed?
- Are sensitive credentials stored locally?
- Can privileges be escalated?
- Is the system joined to Active Directory?
- Which security controls are enabled?
- Can persistence be established?
- Can the host be used for lateral movement?

Understanding Windows as an interconnected operating system is essential for identifying realistic attack paths.

---

# Key Takeaways

- Windows is the primary operating system in enterprise environments.
- Enterprise attacks often begin with a compromised Windows workstation.
- Misconfigurations, excessive privileges, and weak authentication are more commonly exploited than operating system vulnerabilities.
- A strong understanding of Windows architecture provides the foundation for Active Directory exploitation, privilege escalation, persistence, and lateral movement.
