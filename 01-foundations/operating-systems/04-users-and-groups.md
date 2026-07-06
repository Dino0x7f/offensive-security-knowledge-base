# Users and Groups (Security Perspective)

## Introduction

Users and groups form the foundation of identity and access management (IAM) within an operating system.

Every action performed on a system is associated with a security principal, making identity one of the most valuable targets for attackers.

From a security perspective:

> Attackers rarely target machines—they target the identities that control them.

---

# Why Users and Groups Matter

Operating systems use users and groups to control:

- Authentication
- Authorization
- Resource access
- Administrative privileges
- Application permissions
- Network access

Compromising a privileged identity often provides greater value than exploiting the operating system itself.

---

# Users

A user is an identity recognized by the operating system.

Common user types include:

- Standard Users
- Administrative Users
- Service Accounts
- System Accounts
- Guest Accounts (when enabled)

Each user operates within a specific security context and receives permissions based on assigned privileges and group memberships.

---

# Groups

Groups simplify permission management by assigning privileges to multiple users.

Instead of configuring permissions individually, administrators grant permissions to groups.

Examples include:

## Linux

- root
- sudo
- wheel
- docker

## Windows

- Administrators
- Users
- Remote Desktop Users
- Backup Operators
- Remote Management Users

In enterprise environments, Active Directory introduces additional domain groups.

---

# Authentication vs Authorization

## Authentication

Authentication answers:

> Who are you?

Examples:

- Passwords
- SSH Keys
- Kerberos
- NTLM
- Multi-Factor Authentication (MFA)

---

## Authorization

Authorization answers:

> What are you allowed to do?

Authorization determines access to:

- Files
- Services
- Applications
- Administrative functions
- Network resources

---

# Privilege Levels

Operating systems separate users into privilege levels.

## Linux

- Regular User
- root

Administrative access is commonly delegated using:

- sudo
- wheel group

---

## Windows

- Standard User
- Local Administrator
- SYSTEM

In enterprise environments:

- Domain User
- Domain Administrator
- Enterprise Administrator

---

# Security Tokens

After successful authentication, operating systems create a security token representing the user's identity and privileges.

### Security Relevance

Attackers frequently target tokens for:

- Token impersonation
- Privilege escalation
- Lateral movement

---

# Service Accounts

Many applications execute using dedicated service accounts.

Examples include:

- Database services
- Backup services
- Web servers
- Scheduled tasks

### Security Relevance

Weak service account configurations often lead to:

- Privilege escalation
- Credential theft
- Lateral movement

---

# Common Security Weaknesses

Examples include:

- Weak passwords
- Password reuse
- Shared accounts
- Disabled MFA
- Excessive privileges
- Misconfigured sudo rules
- Weak group assignments
- Stale accounts
- Default credentials
- Service accounts with unnecessary privileges

---

# Privilege Escalation

A primary objective after initial access is privilege escalation.

Examples include:

## Linux

- User → root
- sudo misconfiguration
- SUID abuse

## Windows

- User → Administrator
- Administrator → SYSTEM

## Active Directory

- Domain User → Domain Administrator

---

# Attacker Perspective

During enumeration, attackers attempt to identify:

- Local users
- Administrative accounts
- Group memberships
- Service accounts
- Disabled accounts
- Password policies
- Privileged sessions
- Credential reuse opportunities

Identity information frequently determines the fastest path to privilege escalation.

---

# Common Attack Scenarios

Examples include:

- Password spraying
- Credential stuffing
- Pass-the-Hash
- Kerberoasting
- Token impersonation
- Abuse of privileged groups
- Service account compromise
- Sudo misconfiguration

---

# Key Takeaways

- Users represent identities within the operating system.
- Groups simplify privilege management but can also expand the attack surface.
- Authentication verifies identity, while authorization determines permissions.
- Excessive privileges are one of the most common security weaknesses.
- Identity compromise is often the first step toward privilege escalation and full system compromise.
