# Registry Internals (Windows Internals - Security Perspective)

## Overview

The **Windows Registry** is a hierarchical database that stores operating system settings, hardware information, user preferences, installed software configurations, and security-related data.

Unlike Linux, where configuration is mainly stored in text files, Windows centralizes most system configuration inside the Registry.

From a security perspective:

> The Registry is one of the most valuable targets for attackers because it controls persistence, system configuration, software behavior, and user settings.

Understanding Registry internals is essential for malware analysis, persistence techniques, privilege escalation, digital forensics, and Windows administration.

---

# Why the Registry Exists

The Registry provides a centralized configuration database for Windows.

It stores information about:

- Operating system settings
- Installed software
- Device drivers
- Hardware configuration
- User preferences
- Security policies
- Services
- Startup programs

---

# Registry Architecture

```
Windows

      │

 Registry

      │

+----------------+
|     Hives      |
+----------------+

      │

      Keys

      │

   Subkeys

      │

    Values
```

The Registry follows a hierarchical structure similar to a file system.

---

# Registry Components

## Hive

A **Hive** is the top-level storage unit.

Each Hive represents a separate database file.

Examples:

- HKEY_LOCAL_MACHINE
- HKEY_CURRENT_USER
- HKEY_USERS

---

## Key

A Key is similar to a directory.

Example:

```
HKLM

 └── Software

      └── Microsoft

            └── Windows
```

---

## Subkey

A Subkey is simply another Key nested inside a parent Key.

---

## Value

Values store actual data.

Example:

```
Name

↓

Data
```

Different value types exist.

---

# Registry Value Types

| Type | Description |
|-------|-------------|
| REG_SZ | String |
| REG_DWORD | 32-bit Integer |
| REG_QWORD | 64-bit Integer |
| REG_BINARY | Binary Data |
| REG_MULTI_SZ | Multiple Strings |
| REG_EXPAND_SZ | Expandable String |

---

# Major Registry Hives

---

## HKEY_LOCAL_MACHINE (HKLM)

Stores machine-wide settings.

Contains:

- Installed software
- Services
- Drivers
- Security settings
- Hardware configuration

Location:

```
HKLM
```

---

## Security Importance

One of the highest-value Registry locations.

Attackers often modify HKLM for:

- Persistence
- Service creation
- Driver installation

---

## HKEY_CURRENT_USER (HKCU)

Stores settings for the currently logged-in user.

Contains:

- Desktop settings
- Application preferences
- Startup entries

---

## Security Importance

Malware commonly uses HKCU because:

- No administrator rights required
- User-specific persistence

---

## HKEY_USERS (HKU)

Contains Registry data for every user profile.

```
HKU

├── SID1

├── SID2

└── SID3
```

---

## HKEY_CLASSES_ROOT (HKCR)

Stores:

- File associations
- COM Objects
- OLE configuration

---

## Security Importance

Often abused for:

- COM Hijacking
- File association attacks

---

## HKEY_CURRENT_CONFIG (HKCC)

Stores current hardware configuration.

Mostly used by Windows itself.

---

# Physical Registry Storage

Registry Hives are stored on disk.

Examples:

```
C:\Windows\System32\Config\
```

Contains:

- SYSTEM
- SOFTWARE
- SECURITY
- SAM
- DEFAULT

User-specific Registry:

```
NTUSER.DAT
```

Located inside user profiles.

---

# Registry Loading

During boot:

```
Disk

↓

Registry Hive

↓

Memory

↓

Windows Uses Configuration
```

Windows loads Registry Hives into memory.

---

# Common Registry Paths

---

## Startup Programs

```
HKCU\Software\Microsoft\Windows\CurrentVersion\Run

HKLM\Software\Microsoft\Windows\CurrentVersion\Run
```

---

## Services

```
HKLM\SYSTEM\CurrentControlSet\Services
```

---

## Installed Software

```
HKLM\Software
```

---

## User Settings

```
HKCU
```

---

## Network Configuration

```
HKLM\SYSTEM
```

---

# Registry and Persistence

One of the most common persistence techniques.

Example:

```
Run Key

↓

Malware.exe

↓

Executed Every Login
```

---

## Common Persistence Locations

```
Run

RunOnce

RunServices

Shell

Userinit

Winlogon
```

---

# Registry and Services

Windows Services are registered inside:

```
HKLM\SYSTEM\CurrentControlSet\Services
```

Contains:

- Service path
- Startup type
- Permissions
- Dependencies

---

## Security Relevance

Attackers modify services to:

- Gain persistence
- Escalate privileges
- Execute malware

---

# Registry and Drivers

Kernel Drivers are also configured inside the Registry.

Example:

```
HKLM\SYSTEM\CurrentControlSet\Services
```

Driver loading depends heavily on Registry entries.

---

# Registry Permissions

Registry Keys have Access Control Lists (ACLs).

Permissions include:

- Read
- Write
- Full Control

---

## Security Relevance

Weak Registry permissions may allow:

- Privilege escalation
- Configuration tampering
- Persistence

---

# Registry Virtualization

Modern Windows may virtualize Registry writes for legacy applications.

Example:

```
Application

↓

Virtual Registry

↓

Real Registry
```

---

# Registry APIs

Applications interact with the Registry using Windows APIs.

Examples:

```
RegOpenKey()

RegCreateKey()

RegQueryValue()

RegSetValue()

RegDeleteKey()
```

---

# Registry Monitoring

Security tools monitor:

- Key creation
- Value modification
- Startup entries
- Service changes
- Security policy modifications

---

# Registry in Malware

Malware commonly uses the Registry for:

- Persistence
- Configuration storage
- Encryption keys
- C2 settings
- Payload location

---

# Registry Hijacking

Attackers modify Registry Keys to change application behavior.

Example:

```
Application

↓

Registry Lookup

↓

Modified Value

↓

Malicious Execution
```

---

# COM Hijacking

COM Objects are configured in the Registry.

Attackers replace legitimate COM paths with malicious DLLs.

```
Application

↓

COM Lookup

↓

Registry

↓

Malicious DLL
```

---

# Image File Execution Options (IFEO)

Registry path:

```
HKLM\Software\Microsoft\Windows NT\CurrentVersion\Image File Execution Options
```

Allows debugging configuration.

---

## Security Relevance

Attackers abuse IFEO for:

- Persistence
- Application hijacking
- Malware execution

---

# Registry Forensics

During investigations analysts inspect:

- Startup entries
- User activity
- USB history
- Installed software
- Recent files
- Network configuration
- Malware persistence

The Registry is one of the richest forensic artifacts in Windows.

---

# Defensive Perspective

Security solutions monitor:

- Startup keys
- Run keys
- Service creation
- IFEO modifications
- COM registrations
- Security policy changes
- Registry permission changes

Unexpected Registry modifications often indicate compromise.

---

# Pentester Perspective

During an assessment, pentesters inspect the Registry for:

- Stored credentials
- Weak permissions
- Auto-start entries
- Service configurations
- Installed software
- Persistence opportunities
- Misconfigured applications
- Security settings

Many privilege escalation paths originate from Registry misconfigurations.

---

# Relationship with Windows Components

```
Applications

        │

Windows API

        │

Registry

        │

Configuration

        │

Services

Drivers

Users

Security

Software
```

Nearly every Windows component depends on Registry data.

---

# Common Offensive Techniques

| Technique | Registry Usage |
|-----------|----------------|
| Persistence | Run Keys |
| Service Persistence | Services Key |
| COM Hijacking | HKCR |
| IFEO Hijacking | Image File Execution Options |
| Security Policy Changes | HKLM\Security |
| Driver Persistence | Services Hive |
| Configuration Theft | Software Keys |

---

# Key Takeaways

- The Registry is Windows' centralized configuration database.
- Data is organized into Hives, Keys, Subkeys, and Values.
- Major Hives include HKLM, HKCU, HKCR, HKU, and HKCC.
- Registry data controls services, drivers, startup programs, users, and applications.
- Attackers frequently abuse the Registry for persistence, privilege escalation, and stealth.
- Weak Registry permissions can become privilege escalation opportunities.
- Registry analysis is essential for malware analysis, digital forensics, and Windows penetration testing.

---

# Key Insight

> The Windows Registry is far more than a configuration database it is the operating system's control center. Every service, startup program, driver, and user setting ultimately depends on Registry data, making it one of the most critical components for both attackers seeking persistence and defenders investigating system compromise.
