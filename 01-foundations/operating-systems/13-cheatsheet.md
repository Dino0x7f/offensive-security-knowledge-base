# Operating Systems Cheatsheet (Security Perspective)

## Core Components

| Component | Security Relevance |
|----------|--------------------|
| Process | Code execution and process injection |
| Memory | Credential dumping, code injection, buffer overflows |
| File System | Sensitive data, permissions, persistence |
| Users & Groups | Identity, privilege escalation, access control |
| Permissions | Authorization boundaries |
| Services | Local and remote attack surface |
| Networking | Communication channels and exposed services |
| Logs | Detection, auditing, and forensic evidence |

---

# Linux Quick Reference

## Important Directories

| Path | Purpose |
|------|---------|
| `/etc` | System configuration |
| `/etc/passwd` | User accounts |
| `/etc/shadow` | Password hashes |
| `/home` | User home directories |
| `/var/log` | System logs |
| `/tmp` | Temporary files |
| `/root` | Root user's home directory |

### Common Security Risks

- Weak file permissions
- Misconfigured `sudo`
- SUID/SGID binaries
- Writable directories
- Cron job abuse
- Exposed SSH
- Unpatched services

---

# Windows Quick Reference

## Important Components

| Component | Security Relevance |
|----------|--------------------|
| Registry | Configuration and persistence |
| Services | Privilege escalation opportunities |
| LSASS | Credential storage |
| SAM Database | Local account password hashes |
| Scheduled Tasks | Persistence |
| Event Logs | Detection and forensics |
| NTFS ACLs | File access control |

### Common Security Risks

- Weak service permissions
- Credential dumping
- Registry persistence
- Writable service paths
- Weak ACLs
- UAC bypass opportunities
- Unpatched software

---

# Common Privilege Escalation Paths

- User → Root (Linux)
- User → Administrator (Windows)
- Local Administrator → Domain Administrator (Enterprise)

---

# Common Enumeration Targets

- Operating system version
- Installed software
- Running services
- Active processes
- Local users and groups
- Network configuration
- Scheduled tasks
- Startup applications
- Sensitive files
- Credentials and secrets

---

# Common Misconfigurations

- Weak permissions
- Default credentials
- Unpatched software
- Unnecessary services
- Open ports
- Weak authentication
- Insecure scheduled tasks
- Hardcoded credentials
- Misconfigured firewall rules

---

# Pentester Mindset

Always follow this workflow:

1. Enumerate
2. Identify attack surface
3. Find misconfigurations
4. Identify privilege boundaries
5. Escalate privileges
6. Establish persistence
7. Prepare for lateral movement

---

# Key Takeaways

- The operating system enforces identities, permissions, and execution.
- Most compromises result from misconfigurations rather than OS flaws.
- Enumeration is the foundation of every successful assessment.
- Small weaknesses often combine into complete system compromise.

---

# Key Insight

> An operating system is a collection of identities, services, permissions, and trust relationships. A pentester's goal is to understand how those components interact and where they can be abused.
