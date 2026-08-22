# rpcclient

## Overview

rpcclient is a command-line utility from the Samba suite that communicates with Microsoft's Remote Procedure Call (MSRPC) services over SMB. It enables authenticated or anonymous interaction with Windows systems to retrieve information about users, groups, domains, security identifiers (SIDs), password policies, shares, and other Active Directory objects.

Unlike automated tools such as enum4linux or NetExec, rpcclient provides direct access to low-level RPC functions, making it one of the most flexible tools for manual Windows enumeration.

It is widely used during Active Directory assessments, internal penetration tests, Red Team engagements, and SMB reconnaissance.

---

# Core Objectives

rpcclient is designed to answer questions such as:

- Which users exist?
- Which groups exist?
- What is the Domain SID?
- What password policies are configured?
- Which shares are available?
- What domain information can be retrieved?
- Is anonymous RPC access permitted?

Its primary objective is low-level enumeration of Windows systems using Microsoft RPC services.

---

# Architecture

Typical workflow:

```
Target Host

↓

SMB Session

↓

RPC Interface

↓

Authentication

↓

RPC Functions

↓

Windows Information

↓

Attack Surface Analysis
```

Unlike higher-level tools, rpcclient allows direct execution of individual RPC calls.

---

# Supported Services

rpcclient communicates with various Microsoft RPC services, including:

- SAMR (Security Account Manager)
- LSARPC (Local Security Authority)
- SRVSVC (Server Service)
- WKSSVC (Workstation Service)
- NETLOGON
- SPOOLSS

Each service exposes different types of Windows information.

---

# Core Capabilities

## User Enumeration

Retrieves:

- Local users
- Domain users
- User RIDs
- Account information

---

## Group Enumeration

Enumerates:

- Local groups
- Domain groups
- Alias groups
- Built-in groups

---

## SID Enumeration

Retrieves:

- Domain SID
- User SIDs
- Group SIDs

SID enumeration is frequently used during RID cycling attacks.

---

## RID Cycling

Uses sequential Relative Identifiers (RIDs) to discover users and groups.

This technique can reveal valid accounts even when standard enumeration is restricted.

---

## Password Policy Enumeration

Collects:

- Password complexity
- Minimum password length
- Password history
- Lockout threshold
- Maximum password age

These settings assist in evaluating authentication security.

---

## Share Information

Retrieves information about:

- Shared folders
- Administrative shares
- Server configuration

---

## Domain Information

Enumerates:

- Domain name
- Domain SID
- Trust relationships
- Domain controllers
- Server roles

---

# Attack Surface

rpcclient primarily targets:

- SMB (TCP 445)
- NetBIOS (TCP 139)
- Microsoft RPC
- Active Directory
- Windows Servers
- Samba servers

It is effective against both standalone Windows systems and domain environments.

---

# Common Use Cases

rpcclient is commonly used for:

- Windows enumeration
- Active Directory reconnaissance
- RID cycling
- Password policy assessment
- Share discovery
- User discovery
- Domain enumeration
- Internal penetration testing

It is often used when automated tools fail or when fine-grained control is required.

---

# Anonymous Enumeration

Depending on configuration, rpcclient may allow anonymous access to:

- User information
- Domain information
- Password policies
- Shares
- Domain SID

Modern Windows systems usually restrict anonymous RPC access, while legacy systems and Samba deployments may expose significant information.

---

# Detection and Logging

rpcclient activity may be logged by:

- Windows Security Logs
- SMB logs
- RPC service logs
- SIEM platforms
- EDR solutions
- Network monitoring systems

Indicators include:

- SMB session establishment
- RPC requests
- User enumeration
- RID cycling
- Password policy queries

Repeated enumeration activity may trigger security monitoring.

---

# Strengths

rpcclient provides:

- Low-level RPC access
- Flexible manual enumeration
- RID cycling support
- Detailed domain information
- Password policy retrieval
- Active Directory enumeration
- Fine-grained control
- Lightweight operation

Its flexibility makes it invaluable for manual Windows reconnaissance.

---

# Limitations

rpcclient cannot:

- Exploit vulnerabilities
- Perform privilege escalation
- Execute remote commands
- Replace Active Directory graph analysis
- Replace post-exploitation frameworks

Its purpose is information gathering through RPC interfaces.

---

# Comparison with Other Enumeration Tools

| Tool | Primary Purpose |
|------|-----------------|
| rpcclient | Low-level RPC enumeration |
| enum4linux | Automated SMB enumeration |
| NetExec | Enterprise authentication and enumeration |
| smbclient | SMB file access |
| BloodHound | Active Directory relationship analysis |
| Impacket | Protocol implementation framework |

rpcclient provides manual control, whereas tools like enum4linux automate common enumeration tasks.

---

# Relationship to Other Enumeration Tools

rpcclient commonly integrates with:

- enum4linux
- NetExec
- Nmap
- smbclient
- BloodHound
- LDAP enumeration tools
- Kerberos enumeration tools

It frequently serves as a manual verification tool after automated reconnaissance.

---

# Role in Penetration Testing

Typical workflow:

```
Host Discovery

↓

SMB Detection

↓

rpcclient Authentication

↓

User Enumeration

↓

RID Cycling

↓

Password Policy Analysis

↓

Domain Enumeration

↓

Credential Attacks
```

The information gathered helps guide password attacks, privilege escalation, and Active Directory assessments.

---

# Common Commands

| Purpose | Example |
|----------|---------|
| Anonymous Connection | `rpcclient -U "" -N <target>` |
| Authenticated Connection | `rpcclient -U <user> <target>` |
| Enumerate Domain Users | `enumdomusers` |
| Enumerate Domain Groups | `enumdomgroups` |
| Enumerate Local Groups | `enumalsgroups builtin` |
| Display Domain SID | `lsaquery` |
| Query Password Policy | `getdompwinfo` |
| Lookup SID | `lookupsids <SID>` |
| Lookup User RID | `queryuser <RID>` |
| Enumerate Shares | `netshareenum` |

> These commands represent the most common RPC enumeration tasks. Once connected to `rpcclient`, commands are executed from its interactive shell.

---

# Importance in Offensive Security

Understanding rpcclient enables penetration testers to:

- Enumerate Windows environments
- Discover users and groups
- Identify Domain SIDs
- Perform RID cycling
- Assess password policies
- Analyze Active Directory information

Although modern frameworks automate many of these tasks, rpcclient remains an essential tool because it exposes Microsoft's RPC functionality directly, allowing precise and controlled enumeration during Windows security assessments.

---

> **Key Insight:** rpcclient is not an exploitation tool it is a direct interface to Microsoft's RPC services. By exposing low-level Windows management functions, it enables precise enumeration of users, groups, domains, SIDs, and security policies, forming a critical foundation for Active Directory reconnaissance.