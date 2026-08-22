# BloodHound

## Overview

BloodHound is an Active Directory attack path analysis platform that identifies relationships between users, groups, computers, sessions, permissions, and trust relationships within Windows enterprise environments. Rather than focusing on vulnerability scanning or exploitation, BloodHound models Active Directory as a graph and reveals the shortest paths an attacker could use to reach privileged accounts or critical systems.

BloodHound consists of two primary components:

- **SharpHound** – Data collection utility
- **BloodHound** – Graph analysis and visualization interface

By analyzing complex privilege relationships, BloodHound enables security professionals to identify privilege escalation opportunities, lateral movement paths, and misconfigurations that are difficult to detect manually.

It is widely used during Red Team engagements, internal penetration tests, Active Directory security assessments, and Blue Team privilege audits.

---

# Core Objectives

BloodHound is designed to answer questions such as:

- Who can become Domain Admin?
- Which attack paths exist?
- Which users have administrative privileges?
- Where are privileged sessions active?
- Which ACLs create privilege escalation opportunities?
- Which systems are vulnerable to lateral movement?

Its primary objective is graph-based privilege analysis rather than vulnerability discovery.

---

# Architecture

Typical workflow:

```
Active Directory

↓

SharpHound Collection

↓

Collected Data

↓

Graph Database

↓

BloodHound Analysis

↓

Attack Paths

↓

Privilege Escalation Analysis
```

BloodHound transforms Active Directory relationships into a searchable graph.

---

# Components

## SharpHound

Collects Active Directory information, including:

- Users
- Groups
- Computers
- Sessions
- ACLs
- Trusts
- GPOs
- Local Administrators
- RDP permissions
- DCOM permissions
- PSRemote permissions

---

## BloodHound Interface

Provides:

- Interactive graph visualization
- Path analysis
- Built-in queries
- Relationship exploration
- Attack path calculation

---

# Graph Model

BloodHound represents Active Directory as a graph.

Example:

```
User

↓

Group Membership

↓

Administrative Rights

↓

Computer

↓

Session

↓

Domain Admin
```

Nodes represent objects.

Edges represent relationships.

Graph traversal identifies potential attack paths.

---

# Core Capabilities

## Privilege Escalation Analysis

Identifies:

- ACL abuse
- Group nesting
- Delegated permissions
- Object ownership
- Administrative inheritance

---

## Lateral Movement Analysis

Maps:

- Local administrator rights
- Active sessions
- RDP access
- PSRemoting
- DCOM access

---

## Trust Relationship Analysis

Enumerates:

- Domain trusts
- Forest trusts
- Cross-domain permissions

---

## Session Analysis

Identifies:

- Logged-on users
- Privileged sessions
- High-value targets

---

## ACL Analysis

Evaluates:

- GenericAll
- GenericWrite
- WriteOwner
- WriteDACL
- ForceChangePassword
- AddMember
- AddSelf

These permissions frequently create privilege escalation opportunities.

---

# Attack Surface

BloodHound analyzes:

- Active Directory
- Domain Controllers
- Domain Users
- Groups
- Organizational Units
- Computers
- Trusts
- Group Policy Objects
- Access Control Lists

Unlike network scanners, it focuses entirely on identity relationships.

---

# Common Use Cases

BloodHound is commonly used for:

- Active Directory privilege analysis
- Red Team operations
- Blue Team security reviews
- Privilege escalation assessment
- Lateral movement analysis
- Domain takeover simulation
- Identity security auditing

It is considered one of the most important tools for modern Active Directory assessments.

---

# Information Collected

Typical datasets include:

- Users
- Groups
- Computers
- Sessions
- Local administrators
- ACLs
- Trusts
- Organizational Units
- Group Policies
- Kerberos delegation
- Domain configuration

The collected data forms a complete privilege relationship graph.

---

# Detection and Logging

SharpHound collection may generate:

- LDAP queries
- SMB connections
- Session enumeration
- RPC requests
- Active Directory queries

Detection sources include:

- Domain Controller logs
- LDAP monitoring
- SIEM platforms
- Microsoft Defender
- EDR solutions

Large-scale collection activity is generally visible in enterprise environments.

---

# Strengths

BloodHound provides:

- Graph-based analysis
- Automatic attack path discovery
- Privilege escalation visualization
- Relationship mapping
- Enterprise-scale analysis
- Built-in attack queries
- Extensive Active Directory awareness
- Excellent visualization

Its graph model dramatically simplifies complex privilege analysis.

---

# Limitations

BloodHound cannot:

- Exploit vulnerabilities
- Execute remote commands
- Perform credential attacks
- Replace authentication frameworks
- Replace vulnerability scanners

It analyzes relationships rather than exploiting them.

---

# Comparison with Other Active Directory Tools

| Tool | Primary Purpose |
|------|-----------------|
| BloodHound | Graph-based privilege analysis |
| NetExec | Authentication and lateral movement |
| ldapsearch | LDAP enumeration |
| enum4linux | SMB enumeration |
| rpcclient | RPC enumeration |
| PowerView | PowerShell AD enumeration |

BloodHound specializes in relationship analysis rather than data collection alone.

---

# Relationship to Other Enumeration Tools

BloodHound commonly integrates with:

- SharpHound
- NetExec
- ldapsearch
- rpcclient
- enum4linux
- PowerView
- Impacket
- Kerbrute

Enumeration tools gather data, while BloodHound visualizes privilege relationships.

---

# Role in Penetration Testing

Typical workflow:

```
Host Discovery

↓

Credential Acquisition

↓

Active Directory Enumeration

↓

SharpHound Collection

↓

BloodHound Analysis

↓

Attack Path Discovery

↓

Privilege Escalation

↓

Domain Compromise
```

BloodHound frequently determines the most efficient path toward high-value targets.

---

# Common Commands

| Purpose | Example |
|----------|---------|
| Collect All Data | `SharpHound.exe -c All` |
| Collect Session Data | `SharpHound.exe -c Session` |
| Collect ACL Data | `SharpHound.exe -c ACL` |
| Collect Local Admin Data | `SharpHound.exe -c LocalAdmin` |
| Collect Logged-On Users | `SharpHound.exe -c LoggedOn` |
| Collect Group Membership | `SharpHound.exe -c Group` |
| Collect Trust Relationships | `SharpHound.exe -c Trusts` |
| Compress Results | `SharpHound.exe -c All --zipfilename data.zip` |

> SharpHound is responsible for collecting Active Directory data. The generated ZIP file is imported into the BloodHound interface for graph analysis and attack path visualization.

---

# Importance in Offensive Security

Understanding BloodHound enables penetration testers to:

- Identify privilege escalation paths
- Analyze Active Directory permissions
- Discover lateral movement opportunities
- Visualize trust relationships
- Prioritize high-value targets
- Simulate domain compromise scenarios

BloodHound has fundamentally changed Active Directory security assessments by replacing manual privilege analysis with graph-based attack path discovery, allowing security professionals to identify complex relationships that would otherwise remain hidden.

---

> **Key Insight:** BloodHound does not attack Active Directory it models it. By transforming directory relationships into a graph, it reveals the shortest and most effective paths from a low-privileged account to high-value targets, making it one of the most powerful tools for understanding privilege escalation in Windows enterprise environments.