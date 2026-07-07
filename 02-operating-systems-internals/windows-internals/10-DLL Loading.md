# DLL Loading (Windows Internals - Security Perspective)

## Overview

A **DLL (Dynamic Link Library)** is a file containing reusable code and data that multiple programs can share during execution.

Unlike an executable (`.exe`), a DLL cannot run by itself, it must be loaded by another process.

From a security perspective:

> DLL loading is one of the most abused mechanisms in Windows. Attackers frequently exploit the DLL loading process to execute malicious code inside legitimate processes.

Understanding how Windows loads DLLs is essential for malware analysis, privilege escalation, persistence, and defense evasion.

---

# Why DLLs Exist

Instead of embedding the same code into every application, Windows stores common functionality inside DLLs.

Advantages include:

- Code reuse
- Reduced memory usage
- Easier updates
- Modular software design
- Shared system libraries

Example:

```
Application

│

├── kernel32.dll
├── user32.dll
├── advapi32.dll
└── ntdll.dll
```

---

# What is a DLL?

A DLL is a **Portable Executable (PE)** file that contains:

- Functions
- Classes
- Resources
- Global variables

Unlike EXE files:

- DLLs are loaded into another process.
- Execution begins when the host application calls exported functions.

---

# DLL Loading Architecture

```
Application

        │

Windows Loader

        │

Locate DLL

        │

Load into Memory

        │

Resolve Imports

        │

Call DLL Entry Point

        │

Application Uses DLL
```

---

# Windows Loader

The Windows Loader is responsible for:

- Finding the DLL
- Mapping it into memory
- Resolving imports
- Applying relocations
- Calling the DLL entry point

Only after these steps can the application use DLL functions.

---

# DLL Entry Point

Every DLL contains a function named:

```
DllMain()
```

Windows calls it during events such as:

- DLL loaded
- DLL unloaded
- Thread created
- Thread terminated

Example:

```
Load DLL

↓

DllMain()

↓

Initialization
```

---

## Security Relevance

Malware frequently executes malicious code inside **DllMain()**.

This allows code execution immediately after the DLL is loaded.

---

# Static vs Dynamic Loading

## Static Loading

The executable imports the DLL during compilation.

```
Application

↓

Import Table

↓

DLL Loaded Automatically
```

Occurs before program execution.

---

## Dynamic Loading

The application loads the DLL during runtime.

Common APIs:

```
LoadLibrary()

LoadLibraryEx()

GetProcAddress()

FreeLibrary()
```

---

## Security Relevance

Attackers often use dynamic loading to:

- Hide behavior
- Delay execution
- Load payloads only when needed

---

# DLL Search Order

When an application loads a DLL, Windows searches several locations.

Simplified search order:

```
1. Application Directory

2. System32

3. Windows Directory

4. Current Directory

5. PATH Directories
```

The first matching DLL is loaded.

---

# DLL Search Order Hijacking

One of the most common Windows attacks.

Scenario:

```
Application

↓

Needs library.dll

↓

Attacker places fake library.dll

↓

Windows loads attacker's DLL

↓

Malicious Code Executes
```

---

## Security Impact

Results may include:

- Remote Code Execution
- Persistence
- Privilege Escalation

---

# Side-Loading

DLL Side-Loading occurs when a trusted executable loads an attacker-controlled DLL.

Example:

```
Trusted.exe

↓

Loads

↓

Malicious.dll
```

Because the executable is trusted, the malicious DLL also appears legitimate.

---

# DLL Injection

Attackers can inject DLLs into another process.

Typical flow:

```
OpenProcess()

↓

VirtualAllocEx()

↓

WriteProcessMemory()

↓

CreateRemoteThread()

↓

LoadLibrary()

↓

Malicious DLL Loaded
```

---

## Security Relevance

DLL Injection enables:

- Credential stealing
- Keylogging
- API hooking
- Process manipulation

---

# Reflective DLL Loading

Traditional loading relies on the Windows Loader.

Reflective loading bypasses it.

```
Memory

↓

Reflective Loader

↓

DLL Loaded

↓

Execution
```

---

## Advantages

- No file written to disk
- Harder to detect
- Common in post-exploitation frameworks

Examples:

- Cobalt Strike
- Meterpreter
- Beacon

---

# Import Address Table (IAT)

Applications call DLL functions through the Import Address Table.

Example:

```
Application

↓

IAT

↓

kernel32.dll

↓

CreateFile()
```

---

## Security Relevance

Attackers modify the IAT to:

- Redirect function calls
- Hook APIs
- Monitor execution

---

# Export Table

DLLs expose functions through the Export Table.

Example:

```
kernel32.dll

↓

CreateFileA()

ReadFile()

WriteFile()
```

Applications retrieve exported functions using:

```
GetProcAddress()
```

---

# DLL Dependencies

DLLs may depend on other DLLs.

Example:

```
Application

↓

DLL A

↓

DLL B

↓

DLL C
```

Windows automatically loads required dependencies.

---

# KnownDLLs

Windows protects certain critical DLLs through **KnownDLLs**.

Examples:

- kernel32.dll
- ntdll.dll
- user32.dll

These are loaded from trusted system locations.

---

## Security Importance

KnownDLLs reduce DLL hijacking attacks against critical system libraries.

---

# API Hooking

Attackers modify DLL functions to intercept API calls.

Example:

```
Application

↓

CreateFile()

↓

Hooked Function

↓

Malicious Code

↓

Original Function
```

---

## Common Uses

- Keylogging
- Credential theft
- Monitoring
- Evasion

---

# Common Offensive Techniques

## DLL Hijacking

Exploit Windows search order.

---

## DLL Side-Loading

Abuse trusted applications.

---

## DLL Injection

Load malicious DLLs into remote processes.

---

## Reflective DLL Injection

Load DLLs directly into memory.

---

## IAT Hooking

Modify imported function addresses.

---

## Export Address Table Hooking

Modify exported functions.

---

## Manual Mapping

Load a DLL without using the Windows Loader.

Advantages:

- Stealth
- Bypass security monitoring

---

# Defensive Perspective

Security solutions monitor:

- Unexpected DLL loads
- DLLs outside trusted directories
- Unsigned DLLs
- Reflective loading
- Manual mapping
- API hooking
- Suspicious LoadLibrary() usage

Modern EDR products analyze DLL behavior extensively.

---

# Pentester Perspective

During an assessment, pentesters examine:

- DLL search paths
- Writable directories
- Missing DLLs
- DLL side-loading opportunities
- Reflective loading possibilities
- API hooks
- Unsigned libraries

DLL loading is one of the richest sources of privilege escalation and persistence opportunities.

---

# Relationship with Windows Components

```
Application

        │

Windows Loader

        │

DLL Search

        │

LoadLibrary()

        │

DLL

        │

Memory

        │

Execution
```

---

# Common Attack Techniques

| Technique | Description |
|-----------|-------------|
| DLL Hijacking | Exploit search order |
| DLL Side-Loading | Trusted application loads malicious DLL |
| DLL Injection | Inject DLL into another process |
| Reflective DLL Injection | Load DLL without Windows Loader |
| Manual Mapping | Custom DLL loading |
| IAT Hooking | Redirect imported APIs |
| API Hooking | Intercept Windows API calls |

---

# Key Takeaways

- DLLs contain reusable code shared by multiple applications.
- Windows loads DLLs using the Windows Loader.
- `DllMain()` executes when the DLL is loaded.
- Applications can load DLLs statically or dynamically.
- Improper DLL search order can lead to DLL Hijacking.
- DLL Injection and Reflective Loading are common malware techniques.
- Monitoring DLL loading behavior is a key capability of modern EDR solutions.

---

# Key Insight

> DLL loading is a trusted feature designed for modular software, but that same trust makes it a powerful attack surface. Many advanced malware families and post-exploitation frameworks rely on abusing the Windows DLL loading mechanism to achieve stealthy code execution, persistence, and privilege escalation.
