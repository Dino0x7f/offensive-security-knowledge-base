# Enumeration Cheatsheet

## Overview

Enumeration is the process of actively interacting with discovered systems to extract detailed information about services, users, configurations, permissions, and network resources. Unlike reconnaissance, which identifies potential targets, enumeration validates and expands the attack surface by gathering actionable intelligence.

This cheatsheet summarizes the most commonly used enumeration tools for Windows, Active Directory, SMB, LDAP, and enterprise environments.

---

# Enumeration Workflow

```
Live Host

↓

Port Discovery

↓

Service Detection

↓

Protocol Identification

↓

Authentication

↓

Enumeration

↓

Information Collection

↓

Attack Surface Analysis

↓

Exploitation
```

---

# Enumeration Tool Selection

| Tool | Primary Purpose |
|------|-----------------|
| enum4linux | Automated SMB enumeration |
| NetExec | Authentication, AD enumeration, lateral movement |
| CrackMapExec | Legacy authentication and enumeration framework |
| rpcclient | Manual RPC enumeration |
| smbclient | SMB file access and share interaction |
| ldapsearch | Manual LDAP enumeration |
| BloodHound | Graph-based Active Directory analysis |

---

# enum4linux

### Best For

- SMB enumeration
- User discovery
- Group enumeration
- Password policy retrieval
- Share enumeration

### Strengths

- Automated
- Easy to use
- Excellent SMB coverage

### Limitations

- SMB only
- Limited post-authentication capabilities

---

# NetExec (NXC)

### Best For

- Credential validation
- Password spraying
- Pass-the-Hash
- Active Directory enumeration
- Lateral movement

### Strengths

- Active maintenance
- Multi-protocol support
- Enterprise automation

### Limitations

- Requires valid credentials for many features

---

# CrackMapExec

### Best For

- Legacy enterprise assessments
- Credential validation
- SMB enumeration

### Strengths

- Mature workflow
- Multi-host automation

### Limitations

- No longer actively maintained
- Replaced by NetExec

---

# rpcclient

### Best For

- Manual RPC enumeration
- RID cycling
- Password policy analysis
- User and group discovery

### Strengths

- Low-level RPC access
- Fine-grained control

### Limitations

- Interactive
- Requires knowledge of RPC commands

---

# smbclient

### Best For

- Accessing SMB shares
- Downloading files
- Uploading files
- Permission validation

### Strengths

- Lightweight
- Interactive file management

### Limitations

- Focused on file sharing
- Limited enumeration features

---

# ldapsearch

### Best For

- Active Directory queries
- LDAP object enumeration
- User discovery
- Group discovery
- Service account enumeration

### Strengths

- Direct LDAP access
- Flexible search filters

### Limitations

- Manual LDAP syntax
- LDAP only

---

# BloodHound

### Best For

- Privilege escalation analysis
- Attack path discovery
- Active Directory relationship mapping

### Strengths

- Graph visualization
- Automatic attack path calculation

### Limitations

- Requires collected AD data
- Does not exploit vulnerabilities

---

# Common Enumeration Targets

- Windows Hosts
- Domain Controllers
- SMB Servers
- LDAP Servers
- File Shares
- Domain Users
- Groups
- Service Accounts
- Organizational Units
- Active Sessions

---

# Typical Active Directory Enumeration Pipeline

```
Nmap

↓

enum4linux

↓

rpcclient

↓

ldapsearch

↓

NetExec

↓

BloodHound

↓

Privilege Analysis

↓

Exploitation
```

---

# Authentication Methods

| Method | Supported By |
|---------|--------------|
| Anonymous | enum4linux, smbclient, rpcclient, ldapsearch |
| Username/Password | All tools |
| NTLM Hash | NetExec, CrackMapExec |
| Kerberos | NetExec, ldapsearch, smbclient |
| Local Authentication | NetExec |

---

# Information Collected

Enumeration commonly retrieves:

- Users
- Groups
- Shares
- Password Policies
- Domain Information
- Domain SID
- Computer Objects
- Organizational Units
- ACLs
- Trust Relationships
- Sessions
- Local Administrators
- Service Accounts
- SPNs

---

# Common Enumeration Commands

## enum4linux

```bash
enum4linux -a <target>
```

---

## NetExec

```bash
nxc smb <target>
```

```bash
nxc smb <target> -u <user> -p <password>
```

```bash
nxc smb <target> --shares
```

---

## CrackMapExec

```bash
crackmapexec smb <target>
```

---

## rpcclient

```bash
rpcclient -U "" -N <target>
```

Inside the shell:

```text
enumdomusers
enumdomgroups
lsaquery
getdompwinfo
```

---

## smbclient

```bash
smbclient -L //<target> -N
```

```bash
smbclient //<target>/<share> -U <user>
```

Inside the shell:

```text
ls
cd
get
put
```

---

## ldapsearch

```bash
ldapsearch -x -H ldap://<target>
```

```bash
ldapsearch -x -H ldap://<target> -b "DC=example,DC=com"
```

---

## BloodHound (SharpHound)

```powershell
SharpHound.exe -c All
```

```powershell
SharpHound.exe -c Session
```

```powershell
SharpHound.exe -c ACL
```

---

# Enumeration Best Practices

- Enumerate anonymously before authenticating.
- Validate discovered credentials across multiple services.
- Enumerate users before password attacks.
- Review password policies before spraying.
- Inspect accessible SMB shares for sensitive files.
- Query LDAP for domain structure and service accounts.
- Analyze privilege relationships with BloodHound.
- Document every discovered object and permission.

---

# Common Mistakes

- Ignoring anonymous access.
- Skipping password policy enumeration.
- Failing to inspect SMB shares.
- Enumerating only one protocol.
- Ignoring service accounts and SPNs.
- Not validating administrative privileges.
- Running BloodHound without collecting complete data.
- Overlooking trust relationships.

---

# Tool Comparison

| Capability | enum4linux | NetExec | rpcclient | smbclient | ldapsearch | BloodHound |
|------------|------------|----------|-----------|-----------|------------|-------------|
| SMB Enumeration | ✓ | ✓ | Partial | Partial | ✗ | ✗ |
| LDAP Enumeration | ✗ | ✓ | ✗ | ✗ | ✓ | Indirect |
| User Enumeration | ✓ | ✓ | ✓ | ✗ | ✓ | ✓ |
| Group Enumeration | ✓ | ✓ | ✓ | ✗ | ✓ | ✓ |
| Share Enumeration | ✓ | ✓ | Partial | ✓ | ✗ | ✗ |
| Password Policy | ✓ | ✓ | ✓ | ✗ | Partial | ✗ |
| File Access | ✗ | ✗ | ✗ | ✓ | ✗ | ✗ |
| Privilege Analysis | ✗ | Partial | ✗ | ✗ | ✗ | ✓ |
| Attack Path Discovery | ✗ | ✗ | ✗ | ✗ | ✗ | ✓ |

---

# Key Takeaways

- **enum4linux** → Automated SMB reconnaissance.
- **NetExec** → Modern enterprise authentication and enumeration framework.
- **CrackMapExec** → Legacy predecessor of NetExec.
- **rpcclient** → Manual RPC enumeration and RID cycling.
- **smbclient** → SMB file browsing and management.
- **ldapsearch** → Direct LDAP directory queries.
- **BloodHound** → Graph-based privilege escalation and attack path analysis.

---

> **Enumeration Rule:** Reconnaissance tells you *what exists* enumeration tells you *how it works*. Thorough enumeration reveals identities, permissions, configurations, and relationships that transform a discovered service into a practical attack path.