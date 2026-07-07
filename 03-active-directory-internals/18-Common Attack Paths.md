# Common Active Directory Attack Paths 

## Overview

Modern Active Directory attacks rarely rely on software vulnerabilities. Instead, attackers abuse legitimate authentication mechanisms, misconfigurations, excessive privileges, and trust relationships to move from an initial foothold to complete domain compromise.

From a security perspective:

> Most Active Directory compromises follow predictable attack paths. Understanding these paths allows defenders to detect, interrupt, and prevent privilege escalation before attackers reach Tier-0 assets.

Understanding common attack paths is essential for:

- Active Directory Security
- Red Team Operations
- Blue Team Detection
- Privilege Escalation
- Lateral Movement
- Incident Response

---

# What is an Attack Path?

An attack path is the sequence of steps an attacker follows to move from:

```
Initial Access

↓

Privilege Escalation

↓

Credential Theft

↓

Lateral Movement

↓

Domain Compromise
```

Each step exploits trust relationships rather than software vulnerabilities.

---

# Typical Enterprise Attack Chain

```
Phishing

↓

User Workstation

↓

Credential Theft

↓

Lateral Movement

↓

Administrator

↓

Domain Controller

↓

Domain Admin

↓

Persistence
```

---

# Initial Access

Attackers first gain access through:

- Phishing
- Malware
- VPN compromise
- Web application compromise
- Weak passwords
- Exposed RDP
- Third-party compromise

At this stage privileges are usually limited.

---

# Local Privilege Escalation

After compromising a workstation:

Objectives include:

- SYSTEM privileges
- Local Administrator
- Credential dumping
- Service abuse

Common techniques:

- Vulnerable drivers
- Misconfigured services
- Unquoted service paths
- Weak permissions

---

# Credential Access

Attackers seek credentials from:

- LSASS
- SAM
- LSA Secrets
- Cached Credentials
- Browser passwords
- Password managers
- Kerberos tickets

Credential theft is a major turning point in the attack.

---

# Pass-the-Hash

```
NTLM Hash

↓

Authenticate

↓

Remote Access
```

Password knowledge is unnecessary.

Only the NT hash is required.

---

# Pass-the-Ticket

```
Kerberos Ticket

↓

Reuse Ticket

↓

Access Service
```

The attacker authenticates using stolen Kerberos tickets.

---

# Kerberoasting

```
Service Account

↓

Request Service Ticket

↓

Extract Hash

↓

Offline Password Cracking
```

Weak service account passwords are vulnerable.

---

# AS-REP Roasting

Targets accounts that:

```
Do Not Require

Kerberos Pre-Authentication
```

Attackers request encrypted authentication data for offline cracking.

---

# NTLM Relay

```
Victim

↓

Authenticates

↓

Attacker Relays Authentication

↓

Target Server
```

The attacker never learns the password.

Authentication is forwarded.

---

# Token Impersonation

```
SYSTEM Process

↓

Access Token

↓

Duplicate Token

↓

Administrator Context
```

Common on compromised servers.

---

# Lateral Movement

Attackers expand access using:

- SMB
- WinRM
- RDP
- PsExec
- WMI
- Remote Services
- Scheduled Tasks

Objective:

Reach privileged systems.

---

# Delegation Abuse

Targets:

- Unconstrained Delegation
- Constrained Delegation
- Resource-Based Constrained Delegation (RBCD)

Abusing delegation can enable impersonation of privileged users.

---

# Active Directory Enumeration

Attackers identify:

- Domain Admins
- Enterprise Admins
- Trusts
- OUs
- Service Accounts
- GPOs
- ACLs
- Certificate Templates
- Domain Controllers

Enumeration builds the attack graph.

---

# ACL Abuse

Misconfigured permissions may allow:

- Resetting passwords
- Adding group members
- Taking ownership
- Modifying GPOs
- Delegating privileges

ACL abuse often leads directly to privilege escalation.

---

# DCSync

If replication permissions are obtained:

```
Attacker

↓

Requests Replication

↓

Domain Controller

↓

Returns Password Hashes
```

No malware on the Domain Controller is required.

---

# Golden Ticket

Prerequisite:

Compromise of:

```
KRBTGT
```

Attacker creates:

```
Forged TGT

↓

Authenticate

↓

Any Service

↓

Domain Administrator
```

One of the highest-impact attacks.

---

# Silver Ticket

Instead of forging a TGT:

Attackers forge:

```
Service Ticket
```

Only the service account key is required.

---

# AD CS Abuse

Misconfigured Certificate Services enable:

- Certificate enrollment
- PKINIT authentication
- Long-term persistence
- Domain privilege escalation

Common attack families include ESC1–ESC8.

---

# GPO Abuse

If attackers control a Group Policy:

They may:

- Execute code
- Add administrators
- Disable security controls
- Deploy malware
- Establish persistence

A single GPO can affect thousands of systems.

---

# Trust Abuse

Compromised domains may attack:

- Trusted domains
- Child domains
- Parent domains
- Forest trusts

Trusts frequently enable enterprise-wide compromise.

---

# Domain Controller Compromise

Objectives:

- NTDS.dit
- KRBTGT
- Replication rights
- SYSVOL
- Certificate Services

Compromising a Domain Controller usually means full domain compromise.

---

# Persistence

Attackers establish long-term access through:

- Golden Tickets
- Silver Tickets
- AdminSDHolder abuse
- GPOs
- Scheduled Tasks
- Services
- AD CS certificates
- Shadow Credentials
- Backdoor accounts

Persistence often survives password changes.

---

# Typical Attack Graph

```
User

↓

Local Administrator

↓

Credential Dumping

↓

Service Account

↓

Kerberoasting

↓

Domain Administrator

↓

DCSync

↓

KRBTGT

↓

Golden Ticket

↓

Forest Compromise
```

---

# High-Value Targets

| Target | Why It Matters |
|----------|----------------|
| Domain Controllers | Store Active Directory database |
| KRBTGT | Signs Kerberos TGTs |
| Enterprise Admins | Forest-wide privileges |
| Domain Admins | Domain-wide control |
| Certificate Authority | Enterprise trust |
| LSASS | Credentials and tickets |
| NTDS.dit | Password hashes |
| GPOs | Centralized configuration |

---

# Defensive Perspective

Administrators should:

- Implement Tiered Administration
- Enforce Least Privilege
- Protect Domain Controllers
- Secure AD CS
- Audit ACLs
- Monitor Kerberos activity
- Restrict NTLM
- Enable Credential Guard
- Audit replication permissions
- Monitor privileged group membership
- Regularly review attack paths using tools like BloodHound

---

# MITRE ATT&CK Mapping

| Technique | ATT&CK Category |
|------------|-----------------|
| Credential Dumping | Credential Access |
| Kerberoasting | Credential Access |
| DCSync | Credential Access |
| Pass-the-Hash | Lateral Movement |
| Pass-the-Ticket | Lateral Movement |
| Golden Ticket | Persistence / Defense Evasion |
| GPO Abuse | Persistence |
| AD CS Abuse | Privilege Escalation |
| ACL Abuse | Privilege Escalation |

---

# Why Attack Paths Matter

Understanding attack paths enables security professionals to:

- Predict attacker behavior
- Prioritize remediation
- Reduce attack surface
- Detect privilege escalation
- Secure Active Directory infrastructure
- Perform effective threat hunting

---

# Key Takeaways

- Active Directory attacks typically abuse legitimate features rather than software vulnerabilities.
- Common attack paths involve credential theft, privilege escalation, lateral movement, and persistence.
- Techniques such as Kerberoasting, DCSync, Pass-the-Hash, and Golden Tickets are fundamental to modern AD attacks.
- Misconfigurations in ACLs, GPOs, trusts, and AD CS significantly increase enterprise risk.
- Protecting Tier-0 assets and limiting administrative privileges are critical defensive strategies.

---

# Key Insight

> Active Directory compromise is rarely a single event—it is a chain of trusted operations abused in sequence. Attackers succeed by moving through authentication systems, privileges, and trust relationships until they control Tier-0 assets. Understanding these attack paths is essential for breaking the chain before it reaches the Domain Controller.
