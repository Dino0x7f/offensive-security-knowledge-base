# SAM & SECURITY Hive 

## Overview

The **SAM (Security Account Manager)** and **SECURITY** registry hives are two of the most important security databases in Windows.

Together, they store local account information, security policies, and secrets used by the operating system.

From a security perspective:

> The SAM and SECURITY hives represent the local identity database of Windows. Compromising them can allow attackers to recover credential material, enumerate accounts, or abuse stored secrets for privilege escalation and lateral movement.

Understanding these hives is essential for Windows Internals, Digital Forensics, Incident Response, Active Directory, and post-exploitation.

---

# Windows Registry Hives

Windows stores system configuration inside several registry hives.

Important security-related hives include:

```
SYSTEM
SAM
SECURITY
SOFTWARE
DEFAULT
```

Each hive serves a different purpose.

---

# High-Level Architecture

```
Windows Registry

│

├── SYSTEM

├── SAM

├── SECURITY

├── SOFTWARE

└── DEFAULT
```

The SAM and SECURITY hives work closely with LSASS during authentication.

---

# What is the SAM Hive?

The **Security Account Manager (SAM)** is the local Windows account database.

It stores information about:

- Local users
- Local groups
- User identifiers (SIDs)
- Password-related authentication data
- Account configuration

The SAM does **not** store domain users.

---

# Location

Registry location:

```
HKLM\SAM
```

Disk location:

```
C:\Windows\System32\Config\SAM
```

---

# Why SAM Exists

Windows needs a database for:

- Local user accounts
- Local authentication
- Group membership
- Password verification
- Account management

LSASS consults the SAM during local logon.

---

# SAM Authentication Flow

```
User

↓

Winlogon

↓

LSASS

↓

SAM

↓

Verify Account

↓

Access Token
```

---

# Information Stored in SAM

Examples include:

- Username
- RID (Relative Identifier)
- User SID
- Group membership
- Account status
- Password-related authentication data

---

# Local Accounts

Example:

```
Administrator

John

Guest

Support
```

These accounts exist only on the local machine.

---

# Relative Identifier (RID)

Each local account has a RID.

Example:

```
Administrator

RID 500
```

```
Guest

RID 501
```

The RID forms part of the complete SID.

---

# User SID

Example:

```
S-1-5-21-XXXXXXXX-XXXXXXXX-XXXXXXXX-500
```

The final number is the RID.

---

# Local Groups

SAM also stores:

```
Administrators

Users

Remote Desktop Users

Backup Operators
```

Group membership affects authorization decisions.

---

# SAM Protection

The SAM hive is protected by Windows.

Normal users cannot read it directly while the operating system is running.

Access is restricted to SYSTEM-level components.

---

# What is the SECURITY Hive?

The SECURITY hive stores local security configuration and protected secrets used by Windows.

Registry location:

```
HKLM\SECURITY
```

Disk location:

```
C:\Windows\System32\Config\SECURITY
```

---

# Purpose of SECURITY Hive

It stores:

- Local Security Policy
- LSA Secrets
- Cached security information
- Security configuration
- Audit configuration

---

# High-Level Structure

```
SECURITY Hive

├── Policy

├── LSA Secrets

├── Cached Data

└── Security Configuration
```

---

# LSA Secrets

One of the most important components inside the SECURITY hive is:

```
LSA Secrets
```

These are protected values managed by the Local Security Authority (LSA).

Examples may include service-related credentials, cached authentication material, and other protected secrets required by the operating system.

---

# Local Security Authority (LSA)

```
LSASS

↓

LSA

↓

SECURITY Hive

↓

Protected Secrets
```

LSASS manages access to these secrets.

---

# Security Policies

The SECURITY hive stores policies such as:

- Account policies
- Audit settings
- User rights assignments
- Trust information
- Local security configuration

---

# Cached Information

Windows may cache certain authentication-related information to support specific operating scenarios, such as logging on when a domain controller is unavailable.

---

# Relationship Between SAM and SECURITY

```
User Login

↓

LSASS

├── SAM

└── SECURITY

↓

Authentication

↓

Access Token
```

The SAM identifies local accounts, while the SECURITY hive stores security policies and protected secrets.

---

# Relationship with SYSTEM Hive

The SYSTEM hive contains boot and system configuration required by Windows.

```
SYSTEM

↓

Boot Information

↓

LSASS

↓

SAM

↓

SECURITY
```

The three hives work together during authentication.

---

# Authentication Process

```
User

↓

Credentials

↓

LSASS

↓

SAM

↓

SECURITY

↓

Authentication

↓

Access Token
```

---

# Access Control

Windows restricts access to both hives.

Only highly privileged processes can interact with them directly.

This protection reduces the risk of unauthorized disclosure of security information.

---

# Offline Access

If Windows is not running, the registry hive files may be examined as part of:

- Digital forensics
- Incident response
- System recovery

Offline access is one reason technologies such as BitLocker are important for protecting data at rest.

---

# Offensive Perspective

During post-exploitation, attackers are interested in these hives because they contain valuable authentication and security information.

Common objectives include:

- Enumerating local accounts
- Identifying privileged users
- Recovering credential material
- Extracting LSA Secrets
- Gathering information for lateral movement

Modern Windows includes numerous protections that make these activities significantly more difficult.

---

# Defensive Perspective

Security teams should:

- Restrict administrative access
- Enable BitLocker
- Protect LSASS
- Use Credential Guard
- Monitor registry access
- Audit privileged operations
- Detect unauthorized hive access

Protection focuses on preventing attackers from obtaining sensitive security information.

---

# Pentester Perspective

During an assessment, pentesters evaluate:

- Whether registry hives are adequately protected
- Whether administrative access is tightly controlled
- Whether BitLocker is enabled
- Whether Credential Guard is deployed
- Whether sensitive secrets are stored securely
- Whether unnecessary local administrator accounts exist

The objective is to evaluate the resilience of Windows credential storage and local security configuration.

---

# Relationship with Other Windows Components

```
Winlogon

        │

LSASS

        │

├── SAM

├── SECURITY

└── SYSTEM

        │

Access Token

        │

Security Reference Monitor
```

These components collectively establish authentication and authorization.

---

# SAM vs SECURITY

| SAM | SECURITY |
|------|----------|
| Local user database | Local security configuration |
| User accounts | Security policies |
| Local groups | LSA Secrets |
| User SIDs | Cached security information |
| Authentication data | Protected system secrets |

---

# Key Takeaways

- The SAM hive stores local user accounts, groups, SIDs, and authentication-related information.
- The SECURITY hive stores security policies, LSA Secrets, and protected security configuration.
- LSASS relies on both hives during authentication.
- Both hives are protected by Windows and require elevated privileges for access.
- Offline access to these hives is a common focus during digital forensics and incident response.
- Modern Windows security features such as BitLocker and Credential Guard help protect the information they contain.

---

# Key Insight

> The SAM hive defines **who exists** on a Windows system, while the SECURITY hive defines **how those identities are protected**. Together, they form the local trust foundation that LSASS relies on to authenticate users and enforce Windows security.

