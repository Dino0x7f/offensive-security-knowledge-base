# PE (Portable Executable) Format (Windows Internals - Security Perspective)

## Overview

The **Portable Executable (PE)** format is the standard file format used by Windows for executable files, DLLs, drivers, and system binaries.

Common PE files include:

- `.exe`
- `.dll`
- `.sys`
- `.ocx`
- `.scr`

From a security perspective:

> The PE format defines how Windows loads and executes programs. Nearly every malware sample, exploit payload, and legitimate Windows application is ultimately a Portable Executable.

Understanding the PE format is essential for malware analysis, reverse engineering, exploit development, and EDR detection.

---

# Why the PE Format Matters

When a user launches an application:

```
Application.exe

        │

Windows Loader

        │

PE Parser

        │

Memory

        │

Execution Begins
```

The Windows Loader relies entirely on the PE structure.

If the structure is invalid, Windows cannot execute the file.

---

# What is a Portable Executable?

A PE file is a structured binary composed of multiple headers and sections.

These components describe:

- Program architecture
- Required libraries
- Entry point
- Memory layout
- Imports
- Exports
- Resources

---

# High-Level PE Structure

```
+----------------------+
| DOS Header           |
+----------------------+
| DOS Stub             |
+----------------------+
| PE Header            |
+----------------------+
| File Header          |
+----------------------+
| Optional Header      |
+----------------------+
| Section Table        |
+----------------------+
| .text                |
+----------------------+
| .rdata               |
+----------------------+
| .data                |
+----------------------+
| .rsrc                |
+----------------------+
| .reloc               |
+----------------------+
```

---

# DOS Header

Every PE file begins with a DOS Header.

Magic bytes:

```
MZ
```

Example:

```
4D 5A
```

These bytes identify the file as a Windows executable.

---

## Security Relevance

Malware analysts often verify the **MZ signature** to identify PE files.

Many detection engines scan for this signature.

---

# DOS Stub

Immediately after the DOS Header is the DOS Stub.

If executed in DOS:

```
This program cannot be run in DOS mode.
```

Modern Windows ignores most of this section.

---

# PE Signature

After the DOS Stub comes the PE Signature.

Magic bytes:

```
PE\0\0
```

Hex:

```
50 45 00 00
```

This tells Windows where the actual executable structure begins.

---

# COFF File Header

The File Header describes general information.

Contains:

- Machine architecture
- Number of sections
- Compilation timestamp
- File characteristics

Example:

```
x86

or

x64
```

---

# Optional Header

Despite its name, the Optional Header is mandatory for executable images.

It contains critical information including:

- Entry Point
- Image Base
- Section Alignment
- Memory Size
- Subsystem
- DLL Characteristics

---

## Entry Point

The Entry Point is where execution begins.

```
Windows Loader

↓

Entry Point

↓

Program Starts
```

---

## Security Relevance

Packers and malware frequently modify the Entry Point.

Many unpacking techniques begin by recovering the original Entry Point.

---

# Image Base

The preferred virtual memory address where Windows loads the executable.

Example:

```
0x140000000
```

If unavailable, Windows relocates the executable.

---

## Security Relevance

ASLR randomizes the Image Base.

This makes exploitation more difficult.

---

# Data Directories

The Optional Header points to important structures.

Examples:

- Import Table
- Export Table
- Resource Table
- Exception Table
- Certificate Table
- Relocation Table
- TLS Table

These directories allow Windows to locate critical information.

---

# Section Table

The Section Table describes all sections in the executable.

Example:

```
.text

.data

.rdata

.rsrc

.reloc
```

Each section has:

- Name
- Size
- Virtual Address
- Permissions

---

# .text Section

Contains executable machine code.

Permissions:

```
Read

Execute
```

---

## Security Relevance

Attackers frequently inject shellcode into executable regions.

Security tools monitor modifications to the `.text` section.

---

# .data Section

Stores initialized writable data.

Examples:

- Global variables
- Static variables

Permissions:

```
Read

Write
```

---

# .rdata Section

Contains read-only information.

Examples:

- String literals
- Import names
- Constant values

---

# .rsrc Section

Stores application resources.

Examples:

- Icons
- Menus
- Images
- Dialogs
- Version information

---

## Security Relevance

Malware sometimes hides encrypted payloads inside the Resource section.

---

# .reloc Section

Contains relocation information.

Used when Windows loads the executable at a different address.

---

## Security Relevance

Relocations support ASLR.

Without relocations, address randomization becomes limited.

---

# Import Address Table (IAT)

The Import Table lists Windows API functions required by the program.

Example:

```
kernel32.dll

CreateFileA

ReadFile

WriteFile
```

During loading, Windows resolves these addresses.

---

## Security Relevance

Analysts inspect imports to understand program behavior.

Examples:

```
VirtualAlloc()

CreateRemoteThread()

WriteProcessMemory()
```

These APIs often indicate code injection techniques.

---

# Export Table

DLLs expose functions through the Export Table.

Example:

```
kernel32.dll

↓

CreateFileA()

ExitProcess()

LoadLibraryA()
```

Applications use exports to call DLL functions.

---

# Resources

Resources include:

- Icons
- Dialogs
- Fonts
- Bitmaps
- Embedded files

---

## Security Relevance

Attackers may embed:

- Shellcode
- DLLs
- Configuration files
- Encryption keys

inside the Resource section.

---

# TLS (Thread Local Storage)

The PE format supports Thread Local Storage callbacks.

```
Executable

↓

TLS Callback

↓

Entry Point
```

---

## Security Relevance

TLS callbacks execute **before** the Entry Point.

Malware often abuses them to execute code early.

---

# Digital Signature

Executables may include Authenticode signatures.

Purpose:

- Verify publisher
- Detect tampering
- Establish trust

---

## Security Relevance

Attackers may:

- Steal code-signing certificates
- Remove signatures
- Abuse trusted certificates

---

# Windows Loader

The Windows Loader performs the following:

1. Validate PE format
2. Allocate memory
3. Load sections
4. Resolve imports
5. Apply relocations
6. Execute TLS callbacks
7. Jump to Entry Point

Only after these steps does the program begin execution.

---

# Common Offensive Techniques

## Process Hollowing

Replace the executable's `.text` section with malicious code.

---

## Reflective DLL Loading

Load DLLs directly from memory without using the Windows Loader.

---

## Import Table Manipulation

Modify imported APIs.

Often used for:

- API Hooking
- Evasion
- Malware obfuscation

---

## PE Packing

Compress or encrypt executables.

Popular packers:

- UPX
- ASPack
- Custom Packers

Purpose:

- Obfuscation
- Antivirus evasion

---

## PE Injection

Inject malicious sections into legitimate executables.

---

## DLL Search Order Hijacking

Abuse Windows DLL loading behavior.

---

# Defensive Perspective

Security products analyze:

- PE headers
- Section layout
- Import tables
- Entropy
- Digital signatures
- Entry Point anomalies
- Suspicious section permissions

Many static malware detection techniques rely entirely on PE analysis.

---

# Pentester Perspective

When analyzing a PE file, pentesters examine:

- Entry Point
- Imported APIs
- Exported functions
- Compiler information
- Digital signatures
- Resources
- Section permissions
- Packers
- Embedded strings

The PE format often reveals how a program behaves before it is ever executed.

---

# Relationship with Windows Components

```
Executable (.exe)

        │

PE Format

        │

Windows Loader

        │

Memory Manager

        │

Process (EPROCESS)

        │

Threads (ETHREAD)

        │

Execution
```

The PE format serves as the blueprint that the Windows Loader uses to create a running process.

---

# Common Attack Techniques Related to PE Files

| Technique | PE Component Targeted |
|-----------|------------------------|
| Process Hollowing | .text Section |
| DLL Injection | Import Table |
| Reflective Loading | PE Loader |
| API Hooking | IAT |
| PE Packing | Entire Image |
| Resource Injection | .rsrc |
| TLS Callback Abuse | TLS Directory |
| Code Signing Abuse | Certificate Table |

---

# Key Takeaways

- PE is the standard executable format used by Windows.
- Every executable contains structured headers and sections.
- The Windows Loader relies on the PE format to create processes.
- Sections separate executable code, data, resources, and relocation information.
- Import and Export Tables define interactions with Windows APIs.
- Many offensive techniques manipulate PE structures for stealth and code execution.
- Understanding the PE format is fundamental for reverse engineering, malware analysis, and exploit development.

---

# Key Insight

> The Portable Executable format is the blueprint of every Windows program. Before any instruction is executed, the Windows Loader interprets the PE structure to build a running process. For attackers, the PE format offers opportunities for code injection, obfuscation, and evasion; for defenders, it provides one of the richest sources of static analysis and malware detection.
