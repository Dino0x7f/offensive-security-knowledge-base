# Active Directory Cheatsheet

## Core Components

| Component              | Purpose                                    |
| ---------------------- | ------------------------------------------ |
| Forest                 | Highest AD security boundary               |
| Tree                   | Collection of related domains              |
| Domain                 | Administrative and authentication boundary |
| Domain Controller (DC) | Authenticates users and stores AD database |
| OU                     | Logical container for objects              |
| GPO                    | Centralized configuration management       |
| DNS                    | Service discovery for AD                   |
| Global Catalog         | Forest-wide searchable directory           |
| NTDS.dit               | Active Directory database                  |
| SYSVOL                 | Stores GPOs and logon scripts              |

---

# Authentication

| Component | Description |
|----------|-------------|
| Kerberos | Default authentication protocol |
| NTLM | Legacy authentication protocol |
| KDC | Issues Kerberos tickets |
| AS | Authentication Service |
| TGS | Ticket Granting Service |
| TGT | Ticket Granting Ticket |
| Service Ticket | Used to access services |
| PKINIT | Kerberos authentication using certificates |

---

# Important Services

| Service | Purpose |
|----------|----------|
| LDAP | Directory queries |
| LDAPS | Secure LDAP |
| DNS | Domain Controller discovery |
| SMB | File sharing |
| RPC | Remote Procedure Calls |
| WinRM | Remote management |
| RDP | Remote desktop |
| Kerberos | Authentication |
| NTLM | Legacy authentication |

---

# Active Directory Database

| File | Purpose |
|------|----------|
| NTDS.dit | Stores AD objects |
| SYSTEM Hive | Boot key information |
| SECURITY Hive | LSA secrets |
| SAM | Local account database |
| SYSVOL | GPOs & scripts |

---

# Kerberos Flow

```
User

↓

AS

↓

TGT

↓

TGS

↓

Service Ticket

↓

Service
```

---

# NTLM Flow

```
NEGOTIATE

↓

CHALLENGE

↓

AUTHENTICATE
```

---

# Access Token

Contains:

- User SID
- Group SIDs
- Privileges
- Integrity Level
- Default DACL
- Logon SID

---

# Trust Types

| Trust | Description |
|--------|-------------|
| Parent-Child | Automatic, transitive |
| Tree-Root | Between trees |
| Forest | Between forests |
| External | Between domains |
| Shortcut | Optimizes authentication |
| Realm | AD ↔ Kerberos realm |

---

# Replication

| Component | Purpose |
|-----------|----------|
| Multi-Master | All writable DCs replicate |
| USN | Tracks changes |
| Invocation ID | Identifies DC database |
| KCC | Builds replication topology |
| DFSR | Replicates SYSVOL |

---

# FSMO Roles

| Role | Responsibility |
|------|----------------|
| Schema Master | Schema modifications |
| Domain Naming Master | Add/remove domains |
| RID Master | RID allocation |
| PDC Emulator | Password changes & time sync |
| Infrastructure Master | Cross-domain references |

---

# Common LDAP Ports

| Port | Protocol |
|------|----------|
| 389 | LDAP |
| 636 | LDAPS |
| 3268 | Global Catalog |
| 3269 | Secure Global Catalog |

---

# Common Kerberos Ports

| Port | Service |
|------|----------|
| 88 | Kerberos |
| 464 | Password Change |

---

# Common Active Directory Ports

| Port | Service |
|------|----------|
| 53 | DNS |
| 88 | Kerberos |
| 135 | RPC Endpoint Mapper |
| 139 | NetBIOS Session |
| 389 | LDAP |
| 445 | SMB |
| 464 | Kerberos Password Change |
| 636 | LDAPS |
| 3268 | Global Catalog |
| 3269 | Secure Global Catalog |
| 5985 | WinRM HTTP |
| 5986 | WinRM HTTPS |
| 9389 | Active Directory Web Services |

---

# Well-Known Groups

| Group | Privilege |
|--------|-----------|
| Domain Admins | Full domain control |
| Enterprise Admins | Full forest control |
| Schema Admins | Modify schema |
| Administrators | Local administration |
| Account Operators | Manage user accounts |
| Backup Operators | Backup/restore |
| Server Operators | Manage servers |

---

# Common Active Directory Objects

| Object | Description |
|----------|-------------|
| User | Authenticated identity |
| Computer | Domain-joined device |
| Group | Collection of principals |
| OU | Organizational container |
| GPO | Policy object |
| Contact | Directory object |
| Printer | Shared printer |
| Shared Folder | Published resource |

---

# Common SID Prefixes

| SID | Meaning |
|-----|----------|
| S-1-5-18 | Local System |
| S-1-5-19 | Local Service |
| S-1-5-20 | Network Service |
| S-1-5-32-544 | Administrators |
| S-1-5-32-545 | Users |
| S-1-5-32-551 | Backup Operators |

---

# Common Attack Techniques

| Technique | Purpose |
|-----------|----------|
| Pass-the-Hash | Reuse NTLM hash |
| Pass-the-Ticket | Reuse Kerberos ticket |
| Kerberoasting | Crack service account passwords |
| AS-REP Roasting | Crack accounts without pre-auth |
| DCSync | Replicate password hashes |
| Golden Ticket | Forge TGT |
| Silver Ticket | Forge service ticket |
| ACL Abuse | Privilege escalation |
| GPO Abuse | Domain-wide code execution |
| AD CS Abuse | Certificate-based escalation |
| NTLM Relay | Relay authentication |

---

# High-Value Targets

| Target | Why Important |
|----------|---------------|
| Domain Controller | Authentication & directory |
| KRBTGT | Signs Kerberos TGTs |
| NTDS.dit | Password hashes |
| Enterprise CA | PKI trust |
| LSASS | Credentials & tickets |
| SYSVOL | GPOs & scripts |
| AdminSDHolder | Protected object ACLs |

---

# Defensive Priorities

- Protect Domain Controllers
- Secure KRBTGT account
- Audit privileged groups
- Restrict replication permissions
- Harden AD CS
- Limit NTLM usage
- Enable LDAP Signing & Channel Binding
- Enforce Least Privilege
- Protect LSASS (Credential Guard)
- Monitor Kerberos anomalies
- Audit GPO modifications
- Review ACL delegations
- Segment Tier-0 assets

---

# Useful PowerShell Commands

```powershell
Get-ADUser username

Get-ADComputer *

Get-ADGroup "Domain Admins"

Get-ADDomain

Get-ADForest

Get-ADOrganizationalUnit -Filter *

Get-GPO -All

repadmin /replsummary

dcdiag

nltest /dclist:domain.local

klist
```

---

# Red Team Focus

- Kerberos Enumeration
- Service Accounts
- Delegation
- ACL Analysis
- Trust Enumeration
- Certificate Templates
- Replication Permissions
- Tier-0 Assets

---

# Blue Team Focus

- Monitor Event IDs (4624, 4625, 4768, 4769, 4771, 4776)
- Detect DCSync activity
- Audit AD CS
- Monitor privileged group changes
- Review GPO modifications
- Secure replication
- Protect Domain Controllers
- Hunt for abnormal Kerberos behavior

---

# Active Directory Attack Lifecycle

```
Initial Access

↓

Enumeration

↓

Credential Access

↓

Privilege Escalation

↓

Lateral Movement

↓

Domain Admin

↓

DCSync

↓

Persistence

↓

Forest Compromise
```

---

> **Key Insight:** Active Directory security is built on identities, trust relationships, and delegated permissions. Mastering its architecture, authentication protocols, replication mechanisms, and common attack paths is essential for both defending enterprise environments and performing advanced penetration testing.
