# File Systems (Security Perspective)

## Introduction

A file system defines how an operating system stores, organizes, manages, and retrieves data from storage devices.

From a security perspective, the file system is one of the most valuable attack surfaces because it controls access to sensitive information, executable code, configuration files, and system resources.

For attackers, compromising the file system often means compromising the confidentiality, integrity, and availability of the system itself.

---

# Why File Systems Matter in Security

Every operating system relies on its file system to store:

- User data
- System configuration
- Executable programs
- Authentication databases
- Application files
- Logs
- Encryption keys
- Backups

Unauthorized access to these files frequently leads to privilege escalation or complete system compromise.

---

# Common File Systems

## Linux

Common Linux file systems include:

- ext4
- XFS
- Btrfs
- ZFS

Characteristics:

- Hierarchical directory structure
- Case-sensitive
- Permission-based security model
- Everything is treated as a file

---

## Windows

The primary Windows file system is:

- NTFS (New Technology File System)

Characteristics:

- Drive-letter organization (C:\, D:\)
- Access Control Lists (ACLs)
- File ownership
- Alternate Data Streams (ADS)
- Encrypting File System (EFS)

---

# File System Structure

Operating systems organize data into:

- Directories (Folders)
- Files
- Symbolic Links
- Hard Links
- Mount Points

### Security Relevance

Attackers inspect directory structures to locate:

- Sensitive data
- Configuration files
- Credentials
- Executables
- Backup copies

---

# Permissions and Ownership

Every file has associated ownership and access permissions.

## Linux

Permission model:

- Read (r)
- Write (w)
- Execute (x)

Applied to:

- Owner
- Group
- Others

Additional permission mechanisms include:

- SUID
- SGID
- Sticky Bit

---

## Windows

NTFS permissions include:

- Read
- Write
- Modify
- Execute
- Full Control

Permissions are managed through Access Control Lists (ACLs).

---

# Metadata

Every file contains metadata, including:

- Owner
- Creation time
- Modification time
- Permissions
- Size
- Attributes

### Security Relevance

Metadata assists attackers in:

- Timeline reconstruction
- Forensic analysis
- Identifying recently modified files
- Detecting suspicious artifacts

---

# High-Value Files

## Linux

Examples include:

- /etc/passwd
- /etc/shadow
- ~/.ssh/
- /etc/sudoers
- Application configuration files
- Backup archives

---

## Windows

Examples include:

- SAM
- SYSTEM
- SECURITY
- NTUSER.DAT
- Registry hives
- Credential Manager data
- PowerShell history
- Scheduled Task definitions

---

# Common File System Misconfigurations

Frequently encountered weaknesses include:

- World-writable directories
- Weak NTFS ACLs
- Writable executable files
- Exposed backup files
- Sensitive configuration files
- Weak ownership settings
- Insecure shared folders
- Misconfigured network shares

---

# Alternate Data Streams (Windows)

NTFS supports Alternate Data Streams (ADS), allowing additional hidden data to be attached to files.

### Security Relevance

Attackers may abuse ADS to:

- Hide payloads
- Store malicious scripts
- Evade simple file inspections

---

# Symbolic Links and Hard Links

Links reference other files without duplicating data.

### Security Relevance

Improper handling of symbolic links can result in:

- Privilege escalation
- Arbitrary file overwrite
- Local privilege escalation vulnerabilities

---

# Pentester Perspective

During an assessment, penetration testers examine the file system to identify:

- Sensitive documents
- Stored credentials
- SSH keys
- API tokens
- Configuration files
- Backup archives
- Writable directories
- Executable files
- Scheduled task scripts
- Hidden artifacts

The file system frequently provides the information required for privilege escalation and lateral movement.

---

# Common Attack Scenarios

Examples include:

- Reading configuration files containing database passwords
- Exploiting writable service binaries
- Abusing weak ACLs
- Extracting password hashes
- Discovering backup files
- Recovering SSH private keys
- Modifying startup scripts for persistence

---

# Key Takeaways

- File systems store nearly every asset required for system operation.
- Weak permissions are among the most common privilege escalation vectors.
- Sensitive files often provide credentials, secrets, and persistence opportunities.
- Effective file system enumeration is a fundamental skill for every penetration tester.
