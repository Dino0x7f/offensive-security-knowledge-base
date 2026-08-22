# CrackMapExec (CME)

## Overview

CrackMapExec (CME) is a post-exploitation and lateral movement framework designed for assessing the security of Windows Active Directory environments. It combines authentication testing, remote administration, service enumeration, credential validation, and post-exploitation capabilities into a single command-line interface.

Built around common Windows protocols such as SMB, WinRM, MSSQL, LDAP, RDP, FTP, and SSH, CME enables security professionals to rapidly identify credential reuse, validate administrative privileges, enumerate domain information, execute commands remotely, and automate common Active Directory assessment tasks.

Although the original CrackMapExec project is no longer actively maintained, it remains widely recognized within the offensive security community. Its actively maintained successor is **NetExec (nxc)**.

---

# Core Objectives

CrackMapExec is designed to answer questions such as:

- Are the supplied credentials valid?
- Which systems can be authenticated against?
- Where does the user have administrative privileges?
- Which hosts expose SMB or WinRM?
- Can commands be executed remotely?
- What information can be gathered from Active Directory?

Its primary objective is efficient authentication validation and enterprise-wide lateral movement.

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

Command Execution

↓

Credential Collection

↓

Lateral Movement
```

CME automates repetitive Active Directory operations across multiple hosts simultaneously.

---

# Supported Protocols

CrackMapExec supports multiple enterprise protocols, including:

- SMB
- WinRM
- LDAP
- MSSQL
- RDP
- FTP
- SSH
- VNC

Each protocol exposes different enumeration and post-exploitation capabilities.

---

# Core Capabilities

## Authentication Validation

Tests credentials against one or many systems.

Supports:

- Local authentication
- Domain authentication
- NTLM hashes
- Kerberos
- Pass-the-Hash

---

## Administrative Access Discovery

Determines whether authenticated users possess local administrator privileges.

This is one of CME's most valuable features during lateral movement.

---

## SMB Enumeration

Collects information such as:

- Shares
- Sessions
- Logged-on users
- Operating systems
- SMB versions
- Signing configuration

---

## LDAP Enumeration

Enumerates:

- Domain users
- Groups
- Computers
- Organizational Units
- Domain policies
- Trust relationships

---

## Command Execution

Supports remote execution over supported protocols where sufficient privileges exist.

---

## Credential Operations

Supports:

- Password authentication
- NTLM hashes
- Kerberos tickets
- Pass-the-Hash
- Pass-the-Ticket

---

# Attack Surface

CrackMapExec primarily targets:

- Active Directory
- Windows Domains
- SMB services
- WinRM
- LDAP
- Microsoft SQL Server
- Enterprise Windows infrastructure

It is optimized for internal enterprise assessments.

---

# Common Use Cases

CrackMapExec is commonly used for:

- Active Directory enumeration
- Credential validation
- Password spraying
- Pass-the-Hash
- Lateral movement
- Administrative privilege discovery
- SMB auditing
- Internal penetration testing
- Red Team operations

It is often one of the first tools executed after obtaining valid credentials.

---

# Detection and Logging

CME activity may generate:

- Windows Security Events
- Authentication logs
- SMB logs
- WinRM logs
- LDAP queries
- SIEM alerts
- EDR telemetry

Indicators include:

- Multiple authentication attempts
- Remote administrative access
- SMB session creation
- Remote command execution
- Large-scale credential validation

Enterprise monitoring solutions frequently detect CME activity.

---

# Strengths

CrackMapExec provides:

- Multi-host automation
- Fast credential validation
- Integrated enumeration
- Multi-protocol support
- Lateral movement capabilities
- Administrative privilege detection
- Active Directory awareness
- Easy scripting and automation

Its ability to operate across large Windows environments makes it highly effective during enterprise assessments.

---

# Limitations

CrackMapExec cannot:

- Exploit software vulnerabilities directly
- Replace vulnerability scanners
- Perform full Active Directory graph analysis
- Replace BloodHound
- Replace Metasploit for advanced exploitation

It is primarily an authentication, enumeration, and lateral movement framework.

---

# Comparison with Other Tools

| Tool | Primary Purpose |
|------|-----------------|
| CrackMapExec | Authentication, enumeration, and lateral movement |
| NetExec | Modern successor to CME |
| enum4linux | SMB enumeration |
| BloodHound | Active Directory relationship analysis |
| Impacket | Protocol implementation framework |
| PsExec | Remote command execution |

CrackMapExec focuses on automating enterprise-wide operations rather than performing deep protocol analysis.

---

# Relationship to Other Enumeration Tools

CrackMapExec commonly integrates with:

- Nmap
- enum4linux
- BloodHound
- Impacket
- Mimikatz
- Kerbrute
- LDAP enumeration tools
- PowerView

Information gathered from these tools often becomes input for CME authentication and lateral movement workflows.

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

Remote Enumeration

↓

Command Execution

↓

Lateral Movement
```

CME significantly reduces the manual effort required to assess large Windows environments.

---

# Common Commands

| Purpose | Example |
|----------|---------|
| SMB Enumeration | `crackmapexec smb <target>` |
| Authenticate with Username & Password | `crackmapexec smb <target> -u <user> -p <password>` |
| Pass-the-Hash | `crackmapexec smb <target> -u <user> -H <NTLM_hash>` |
| Check Local Admin Access | `crackmapexec smb <target> -u <user> -p <password> --local-auth` |
| Enumerate Shares | `crackmapexec smb <target> --shares` |
| Enumerate Logged-on Users | `crackmapexec smb <target> --sessions` |
| Execute Remote Command | `crackmapexec smb <target> -x "<command>"` |
| LDAP Enumeration | `crackmapexec ldap <domain_controller>` |
| WinRM Authentication | `crackmapexec winrm <target> -u <user> -p <password>` |

> These commands represent the most common Active Directory assessment tasks. Modern engagements typically use **NetExec (nxc)**, which maintains nearly identical syntax while adding new capabilities and ongoing support.

---

# Importance in Offensive Security

Understanding CrackMapExec enables penetration testers to:

- Validate credentials efficiently
- Discover administrative access
- Enumerate Windows environments
- Assess Active Directory security
- Automate lateral movement
- Scale internal security assessments

Although NetExec has become the actively maintained successor, CrackMapExec established the standard workflow for automated Active Directory enumeration and remains one of the most influential tools in Windows offensive security.

---

> **Key Insight:** CrackMapExec is not simply an SMB tool it is an Active Directory operations framework. By combining authentication, enumeration, privilege validation, and remote administration across multiple Windows protocols, it enables rapid assessment of enterprise environments and dramatically accelerates lateral movement analysis.