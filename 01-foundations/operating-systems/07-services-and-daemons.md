# Services and Daemons (Security Perspective)

## Introduction

Services (Windows) and daemons (Linux) are background processes designed to perform system functions without direct user interaction.

They provide essential functionality such as authentication, networking, logging, scheduling, remote access, and application hosting.

From a security perspective:

> Services represent one of the largest attack surfaces in modern operating systems because they often run continuously with elevated privileges.

---

# Why Services Matter

Almost every operating system depends on services to:

- Start system components
- Provide network services
- Manage authentication
- Run scheduled tasks
- Host applications
- Monitor system activity

Compromising a privileged service often provides attackers with elevated access or persistence.

---

# Linux Daemons

In Linux, background services are commonly referred to as **daemons**.

Examples include:

- sshd
- cron
- systemd
- nginx
- apache2
- mysqld

Most modern Linux distributions manage daemons using **systemd**.

---

# Windows Services

Windows services are background applications managed by the **Service Control Manager (SCM)**.

Examples include:

- Windows Spooler
- WinRM
- Remote Registry
- MSSQL Server
- IIS Services

Services may start automatically during boot or when required by the operating system.

---

# Service Lifecycle

Services typically transition through several states:

- Installed
- Starting
- Running
- Paused
- Stopped
- Restarting

Understanding service states is useful during troubleshooting, incident response, and penetration testing.

---

# Startup Types

Services can be configured to start in different ways.

Common startup types include:

- Automatic
- Automatic (Delayed Start)
- Manual
- Disabled

### Security Relevance

Unnecessary automatically started services increase the system's attack surface.

---

# Service Accounts

Services execute using specific security identities.

Examples include:

## Linux

- root
- www-data
- mysql
- nobody

## Windows

- LocalSystem
- LocalService
- NetworkService
- Custom Service Accounts

### Security Relevance

Service accounts frequently possess elevated privileges and become valuable targets during privilege escalation.

---

# Why Services Matter in Security

Services often:

- Run with elevated privileges
- Execute automatically
- Listen on network ports
- Interact with sensitive resources
- Accept remote connections

A vulnerable service may expose the entire operating system.

---

# Common Security Risks

## Exposed Services

Examples include:

- SSH
- RDP
- SMB
- FTP
- Database services

Poorly secured network services provide opportunities for initial access.

---

## Misconfigured Services

Examples include:

- Writable service binaries
- Weak service permissions
- Insecure service paths
- Excessive privileges
- Weak authentication

These weaknesses frequently lead to privilege escalation.

---

## Vulnerable Services

Outdated or unpatched services may contain publicly known vulnerabilities that allow:

- Remote Code Execution (RCE)
- Privilege Escalation
- Information Disclosure
- Denial of Service (DoS)

---

## Service Account Abuse

Attackers frequently target privileged service accounts to:

- Extract credentials
- Move laterally
- Execute privileged commands
- Maintain persistence

---

# Services as an Attack Surface

During enumeration, attackers typically identify:

- Running services
- Startup configuration
- Listening ports
- Service versions
- Service accounts
- Executable paths
- File permissions
- Registry configuration (Windows)

Every discovered service represents a potential attack vector.

---

# Services and Persistence

Services are commonly abused for persistence because they:

- Start automatically after reboot
- Often execute with elevated privileges
- Blend into normal system activity

Malware frequently installs itself as a legitimate-looking service.

---

# Attacker Perspective

Attackers attempt to determine:

- Which services run with administrative privileges?
- Which services expose network interfaces?
- Can service binaries be modified?
- Can service configurations be changed?
- Are vulnerable versions installed?
- Which services can be abused for persistence?

Service enumeration is a standard step during post-exploitation.

---

# Common Attack Scenarios

Examples include:

- Exploiting vulnerable network services
- Unquoted service path attacks
- Writable service binary replacement
- Weak service permissions
- Remote service exploitation
- Service account compromise
- Malicious service installation for persistence

---

# Key Takeaways

- Services and daemons provide critical operating system functionality.
- They frequently run with elevated privileges and execute automatically.
- Misconfigured or vulnerable services are common privilege escalation vectors.
- Service enumeration is an essential part of penetration testing.
- Every unnecessary running service increases the system's attack surface.
