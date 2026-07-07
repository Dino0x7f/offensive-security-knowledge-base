# Windows Internals Cheatsheet

## Core Components

| Component | Purpose | Security Relevance |
|----------|---------|-------------------|
| Kernel | Core OS | Highest privilege level |
| Executive | System services | Resource management |
| Object Manager | Manages Windows objects | Handle/Object security |
| Memory Manager | Virtual memory | Memory exploitation |
| Process Manager | Process lifecycle | Process injection |
| I/O Manager | Device communication | Driver attacks |
| SRM | Authorization | Access control |

---

## Execution Modes

| User Mode | Kernel Mode |
|------------|------------|
| Limited privileges | Full privileges |
| Applications | Kernel & Drivers |
| Cannot access hardware directly | Direct hardware access |
| Easier to isolate crashes | Crash affects entire system |

---

## Boot Process

| Order | Component |
|------|-----------|
| 1 | UEFI / BIOS |
| 2 | Windows Boot Manager |
| 3 | Winload |
| 4 | ntoskrnl.exe |
| 5 | HAL |
| 6 | SMSS |
| 7 | Wininit |
| 8 | LSASS |
| 9 | SCM |
| 10 | Winlogon |
| 11 | Explorer |

---

## Processes

| Component | Description |
|-----------|-------------|
| EPROCESS | Kernel process object |
| Thread | Execution unit |
| Handle Table | Object references |
| Access Token | Security context |
| Virtual Memory | Private address space |

---

## Memory

| Region | Purpose |
|---------|---------|
| Stack | Function calls |
| Heap | Dynamic allocation |
| Image | Executable |
| DLL | Shared libraries |

---

## PE Structure

| Section | Purpose |
|----------|---------|
| DOS Header | Legacy compatibility |
| PE Header | Metadata |
| .text | Executable code |
| .data | Variables |
| .rdata | Read-only data |
| .rsrc | Resources |

---

## Authentication

| NTLM | Kerberos |
|------|-----------|
| Legacy | Modern |
| Challenge/Response | Ticket-based |
| No Mutual Auth | Mutual Authentication |
| Local & Legacy | Active Directory |

---

## Registry Hives

| Hive | Purpose |
|------|---------|
| SYSTEM | Boot configuration |
| SAM | Local accounts |
| SECURITY | LSA Secrets & Policies |
| SOFTWARE | Installed software |
| DEFAULT | Default profile |

---

## Access Token

Contains:

- User SID
- Group SIDs
- Privileges
- Integrity Level
- Default DACL

---

## LSASS Responsibilities

- Authentication
- Kerberos
- NTLM
- Access Token creation
- Local Security Policy
- Credential management

---

## SCM

| Item | Description |
|------|-------------|
| Process | services.exe |
| Registry | HKLM\SYSTEM\CurrentControlSet\Services |
| Purpose | Manage Windows Services |

---

## Important Processes

| Process | Function |
|----------|----------|
| System | Kernel |
| smss.exe | Session Manager |
| csrss.exe | Runtime subsystem |
| wininit.exe | Windows initialization |
| services.exe | SCM |
| lsass.exe | Authentication |
| winlogon.exe | Interactive logon |
| explorer.exe | Windows shell |

---

## Common Attack Targets

| Component | Common Abuse |
|------------|--------------|
| LSASS | Credential dumping |
| Access Tokens | Token impersonation |
| Services | Privilege escalation |
| Registry | Persistence |
| DLL Loading | DLL Hijacking |
| SAM | Credential extraction |
| SECURITY Hive | LSA Secrets |
| Handles | Process access |
| Virtual Memory | Code injection |

---

## Common Persistence

| Technique | Component |
|------------|-----------|
| Services | SCM |
| Registry Run Keys | Registry |
| Startup Folder | Explorer |
| Scheduled Tasks | Task Scheduler |
| WMI | Event Subscription |

---

## Enumeration Priority

| Priority | Target |
|----------|--------|
| 1 | Processes |
| 2 | Services |
| 3 | Privileges |
| 4 | Access Tokens |
| 5 | Registry |
| 6 | Scheduled Tasks |
| 7 | File Permissions |
| 8 | Network Services |
| 9 | Authentication |
| 10 | Trust Relationships |

---

# Pentester Mindset

> Windows Internals is not about memorizing Windows components it is about understanding where trust exists, how execution flows, and which boundaries can be abused to gain code execution, escalate privileges, or maintain persistence.
