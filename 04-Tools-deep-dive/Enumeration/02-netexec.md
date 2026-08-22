# NetExec (NXC)

## Overview

NetExec (NXC) is a modern post-exploitation and lateral movement framework for Windows Active Directory environments. It is the actively maintained successor to CrackMapExec (CME), providing enhanced protocol support, improved stability, better performance, and continuous feature development.

NetExec enables security professionals to authenticate against multiple systems, enumerate Active Directory, validate credentials, identify administrative access, execute commands remotely, interact with enterprise services, and automate large-scale internal assessments.

Designed for penetration testing, Red Team operations, Active Directory security assessments, and purple team engagements, NetExec has become one of the primary tools for Windows infrastructure enumeration and post-authentication operations.

---

# Core Objectives

NetExec is designed to answer questions such as:

- Are credentials valid?
- Where does this account have administrative access?
- Which services are exposed?
- What information can be gathered from Active Directory?
- Can commands be executed remotely?
- Which systems are vulnerable to credential reuse?
- How can lateral movement be automated?

Its primary objective is enterprise-wide authentication, enumeration, and post-authentication assessment.

---

# Architecture

Typical workflow:

```
Target Hosts

↓

Protocol Connection

↓

Authentication

↓

Privilege Validation

↓

Enumeration

↓

Credential Operations

↓

Command Execution

↓

Lateral Movement
```

NetExec automates repetitive operations across large enterprise environments.

---

# Supported Protocols

NetExec supports numerous enterprise protocols, including:

- SMB
- WinRM
- LDAP
- MSSQL
- RDP
- SSH
- FTP
- VNC
- WMI

Each protocol exposes specialized enumeration and post-authentication capabilities.

---

# Core Capabilities

## Authentication Validation

Supports:

- Username and password authentication
- NTLM authentication
- Kerberos authentication
- Pass-the-Hash
- Pass-the-Ticket
- Local authentication

Credential validation can be performed against hundreds of hosts simultaneously.

---

## Administrative Access Discovery

Determines whether authenticated users possess local administrator privileges on remote systems.

This capability is fundamental during lateral movement assessments.

---

## SMB Enumeration

Collects information such as:

- Shares
- Logged-on users
- Sessions
- SMB signing
- Operating systems
- SMB versions
- Host information

---

## LDAP Enumeration

Retrieves:

- Users
- Groups
- Computers
- Organizational Units
- Domain policies
- Trust relationships
- Domain controllers

---

## Remote Command Execution

Supports remote command execution over supported protocols where sufficient privileges exist.

---

## Credential Operations

Supports:

- Password authentication
- NTLM hashes
- Kerberos tickets
- Credential spraying
- Pass-the-Hash
- Token-based authentication

---

## Module Framework

NetExec includes modular functionality for:

- Credential collection
- Domain enumeration
- Configuration auditing
- Security assessments
- Remote administration

Modules simplify complex Active Directory operations.

---

# Attack Surface

NetExec primarily targets:

- Windows Active Directory
- SMB services
- WinRM
- LDAP
- Microsoft SQL Server
- Enterprise Windows hosts
- Domain Controllers

It is optimized for internal Windows environments.

---

# Common Use Cases

NetExec is commonly used for:

- Active Directory enumeration
- Credential validation
- Password spraying
- Pass-the-Hash
- Pass-the-Ticket
- Lateral movement
- Administrative privilege discovery
- SMB auditing
- Internal penetration testing
- Red Team operations

It often becomes the central framework after obtaining initial credentials.

---

# Detection and Logging

NetExec activity may generate:

- Windows Security Events
- Authentication logs
- SMB logs
- WinRM logs
- LDAP queries
- SIEM alerts
- Microsoft Defender alerts
- EDR telemetry

Indicators include:

- Multiple authentication attempts
- Remote command execution
- SMB session creation
- WinRM activity
- LDAP enumeration
- Credential spraying

Modern enterprise monitoring solutions frequently detect NetExec activity.

---

# Strengths

NetExec provides:

- Active maintenance
- Multi-protocol support
- Fast authentication validation
- Enterprise-scale automation
- Modular architecture
- Improved stability
- Extensive Active Directory awareness
- Better performance than CrackMapExec

Its modern architecture makes it the preferred framework for Windows enterprise assessments.

---

# Limitations

NetExec cannot:

- Exploit software vulnerabilities directly
- Replace vulnerability scanners
- Replace BloodHound relationship analysis
- Replace Metasploit exploitation modules
- Bypass authentication mechanisms without valid credentials

Its primary focus remains authenticated enumeration and post-authentication operations.

---

# Comparison with Other Tools

| Tool | Primary Purpose |
|------|-----------------|
| NetExec | Modern AD enumeration and lateral movement |
| CrackMapExec | Legacy predecessor |
| enum4linux | SMB enumeration |
| BloodHound | Active Directory relationship analysis |
| Impacket | Protocol implementation framework |
| PsExec | Remote administration |

NetExec extends the CrackMapExec workflow while providing ongoing development and broader protocol support.

---

# Relationship to Other Enumeration Tools

NetExec commonly integrates with:

- Nmap
- enum4linux
- BloodHound
- Impacket
- Mimikatz
- Kerbrute
- PowerView
- LDAP enumeration tools

These tools complement NetExec by providing infrastructure discovery, credential acquisition, or graph-based privilege analysis.

---

# Role in Penetration Testing

Typical workflow:

```
Host Discovery

↓

SMB Enumeration

↓

Credential Acquisition

↓

Credential Validation

↓

Administrative Access Discovery

↓

LDAP Enumeration

↓

Remote Command Execution

↓

Lateral Movement

↓

Domain Compromise Assessment
```

NetExec significantly accelerates authenticated assessments across enterprise Windows environments.

---

# Common Commands

| Purpose | Example |
|----------|---------|
| SMB Enumeration | `nxc smb <target>` |
| Authenticate with Username & Password | `nxc smb <target> -u <user> -p <password>` |
| Pass-the-Hash | `nxc smb <target> -u <user> -H <NTLM_hash>` |
| Local Authentication | `nxc smb <target> --local-auth -u <user> -p <password>` |
| Enumerate Shares | `nxc smb <target> --shares` |
| Enumerate Sessions | `nxc smb <target> --sessions` |
| Enumerate Logged-on Users | `nxc smb <target> --loggedon-users` |
| Execute Remote Command | `nxc smb <target> -x "<command>"` |
| LDAP Enumeration | `nxc ldap <domain_controller>` |
| WinRM Authentication | `nxc winrm <target> -u <user> -p <password>` |
| Password Spraying | `nxc smb <targets> -u users.txt -p '<password>'` |

> NetExec maintains a command syntax very similar to CrackMapExec while introducing new modules, improved protocol support, and active maintenance, making it the recommended framework for modern Active Directory assessments.

---

# Importance in Offensive Security

Understanding NetExec enables penetration testers to:

- Validate credentials efficiently
- Discover administrative access
- Enumerate Active Directory
- Automate enterprise assessments
- Assess credential reuse
- Perform controlled lateral movement
- Scale Windows security testing

NetExec has become one of the most important frameworks for authenticated Windows assessments because it consolidates authentication, enumeration, privilege validation, and remote administration into a single, actively maintained platform.

---

> **Key Insight:** NetExec is the evolution of CrackMapExec. Rather than being a single-purpose SMB tool, it serves as a comprehensive Active Directory operations framework that enables security professionals to authenticate, enumerate, validate privileges, and automate post-authentication assessments across large Windows enterprise environments.