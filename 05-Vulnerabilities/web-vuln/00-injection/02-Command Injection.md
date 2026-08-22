# Command Injection

## Overview

Command Injection (OS Command Injection) is a vulnerability that occurs when an application executes operating system commands using untrusted user input without proper validation or separation between data and commands.

Unlike SQL Injection, which targets a database engine, Command Injection targets the underlying operating system. Successful exploitation may allow attackers to execute arbitrary system commands with the privileges of the vulnerable application.

Because it directly interacts with the operating system, Command Injection is often considered one of the most severe web vulnerabilities and can lead to complete server compromise.

---

# How Command Injection Works

Many applications interact with the operating system to perform legitimate tasks.

Typical flow:

```
User Input

↓

Application

↓

Operating System Command

↓

OS Shell

↓

Command Execution

↓

Application Response
```

If user input becomes part of the executed command, an attacker may inject additional commands.

---

## Example

Application executes:

```bash
ping example.com
```

User input:

```text
example.com
```

Generated command:

```bash
ping example.com
```

If the application concatenates user input directly:

```text
example.com && whoami
```

Generated command:

```bash
ping example.com && whoami
```

The shell interprets both commands.

---

# Root Cause

Command Injection occurs when applications fail to separate user-controlled data from executable operating system commands.

Common causes include:

- Dynamic shell command construction
- Unsafe system() calls
- exec() with user input
- shell_exec()
- popen()
- Runtime.exec()
- Process spawning without argument separation
- Poor input validation

---

# Attack Surface

Command Injection commonly appears in features that interact with the operating system.

Examples include:

- Network diagnostic tools
- Ping utilities
- Traceroute
- DNS lookup
- Image conversion
- File compression
- Backup utilities
- Log management
- Video processing
- PDF generation

---

# Common Targets

Command Injection affects applications written in almost every programming language.

Examples include:

- PHP
- Python
- Java
- Node.js
- Ruby
- Perl
- Go
- C/C++

The vulnerability is independent of language and depends on unsafe interaction with the operating system.

---

# Types of Command Injection

## Direct Command Injection

Injected commands execute immediately.

---

## Blind Command Injection

The application does not display command output.

Attackers infer execution through:

- Time delays
- External callbacks
- Side effects

---

## Out-of-Band Command Injection

Results are delivered through another communication channel.

Examples:

- DNS requests
- HTTP callbacks
- ICMP traffic

---

# Common Command Separators

Attackers commonly abuse shell operators.

| Operator | Purpose |
|----------|----------|
| `;` | Execute next command |
| `&&` | Execute if previous succeeds |
| `||` | Execute if previous fails |
| `|` | Pipe output |
| `&` | Background execution |
| `` ` ` `` | Command substitution |
| `$()` | Command substitution |

Supported operators vary depending on the operating system and shell.

---

# Potential Impact

Successful exploitation may allow attackers to:

- Execute arbitrary commands
- Read sensitive files
- Modify files
- Delete files
- Download malware
- Upload web shells
- Enumerate the operating system
- Access credentials
- Pivot inside the network
- Achieve Remote Code Execution

The impact depends on the privileges of the vulnerable application.

---

# Common Indicators

Possible indicators include:

- Unexpected command output
- Server-side delays
- Modified system files
- Unexpected network traffic
- Process creation anomalies
- Security monitoring alerts

---

# Mitigation

The safest approach is to avoid invoking the system shell whenever possible.

Recommended mitigations include:

- Avoid shell execution
- Use safe system APIs
- Pass arguments separately
- Allow-list input validation
- Least-privileged execution
- Sandboxing
- Input normalization
- Secure coding practices

Applications should treat all user input as untrusted data.

---

# Detection Methods

Security professionals identify Command Injection through:

- Manual testing
- Source code review
- Dynamic Application Security Testing (DAST)
- Static Application Security Testing (SAST)
- Fuzzing
- Runtime analysis
- Automated vulnerability scanners

---

# Command Injection vs Remote Code Execution

Although closely related, they are different concepts.

| Command Injection | Remote Code Execution |
|-------------------|----------------------|
| Executes OS commands | Executes arbitrary program code |
| Requires unsafe shell interaction | May result from many vulnerability types |
| Often leads to RCE | RCE is the final outcome |

Command Injection is one possible path to Remote Code Execution.

---

# Relationship to Other Vulnerabilities

Command Injection frequently appears in attack chains.

Example:

```
Command Injection

↓

Operating System Access

↓

Credential Discovery

↓

Privilege Escalation

↓

Persistence

↓

Complete Server Compromise
```

---

# Real-World Examples

Command Injection has affected:

- Network management interfaces
- Routers
- IoT devices
- Web administration panels
- Backup software
- Monitoring systems
- CI/CD platforms

Many critical vulnerabilities have resulted from unsafe shell command execution.

---

# Importance in Offensive Security

Understanding Command Injection enables penetration testers to:

- Assess operating system interaction
- Evaluate server-side functionality
- Identify unsafe process execution
- Simulate realistic attack scenarios
- Demonstrate full system compromise
- Recommend secure coding practices

---

## Prerequisites

Before studying Command Injection, you should understand:

- Operating System Fundamentals
- Linux Command Line Basics
- Process Execution
- Web Fundamentals
- SQL Injection
- NoSQL Injection

---

## Next Step

Continue with:

**03-LDAP Injection.md**

This chapter explores LDAP Injection, where attackers manipulate Lightweight Directory Access Protocol (LDAP) queries to bypass authentication, enumerate directory services, or retrieve unauthorized directory information.

---

> **Key Insight:** Command Injection occurs when applications confuse user-controlled data with operating system commands. The most effective defense is to avoid invoking the system shell and to treat all external input as untrusted throughout the command execution process.