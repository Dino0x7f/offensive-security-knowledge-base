# Linux Internals Cheatsheet (Security Perspective)

## Linux Architecture

| Component | Purpose | Security Relevance |
|-----------|---------|--------------------|
| User Space | Runs applications | Initial code execution |
| Kernel Space | Core operating system | Highest privilege level |
| System Calls | Interface to kernel | Privileged operations |
| Device Drivers | Hardware communication | Driver vulnerabilities |
| Hardware | Physical resources | Managed by kernel |

---

## Boot Process

| Stage | Responsibility |
|--------|----------------|
| BIOS / UEFI | Hardware initialization |
| Bootloader (GRUB) | Loads kernel |
| Linux Kernel | Initializes operating system |
| initramfs | Temporary root filesystem |
| systemd (PID 1) | Starts userspace |
| Services | Background processes |
| Login | User authentication |

---

## Processes & Threads

| Component | Description |
|-----------|-------------|
| Process | Running program |
| Thread | Execution unit inside a process |
| PID | Process Identifier |
| PPID | Parent Process ID |
| Scheduler | Allocates CPU time |
| File Descriptors | Open resources |

---

## Virtual Memory

| Region | Purpose |
|---------|----------|
| Code (.text) | Executable instructions |
| Data | Global variables |
| Heap | Dynamic memory |
| Stack | Function calls |
| Shared Libraries | External code |
| Kernel Space | OS memory |

---

## Memory Protections

| Protection | Purpose |
|------------|---------|
| ASLR | Randomizes memory layout |
| NX | Prevents code execution in data pages |
| PIE | Randomizes executable location |
| RELRO | Protects GOT |
| Stack Canary | Detects stack corruption |

---

## /proc Filesystem

| Path | Purpose |
|------|---------|
| /proc/<PID>/cmdline | Process arguments |
| /proc/<PID>/exe | Executable path |
| /proc/<PID>/maps | Memory layout |
| /proc/<PID>/fd | Open file descriptors |
| /proc/<PID>/status | Process metadata |
| /proc/cpuinfo | CPU information |
| /proc/meminfo | Memory information |
| /proc/version | Kernel version |
| /proc/net | Network information |

---

## /sys Filesystem

| Path | Purpose |
|------|---------|
| /sys/class | Device classes |
| /sys/devices | Hardware devices |
| /sys/block | Storage devices |
| /sys/module | Loaded kernel modules |
| /sys/kernel | Kernel information |
| /sys/fs | Filesystem subsystems |
| /sys/class/net | Network interfaces |

---

## systemd

| Component | Purpose |
|-----------|---------|
| PID 1 | First userspace process |
| Service Units | Background services |
| Targets | System states |
| Journald | Logging |
| Timers | Scheduled execution |
| Socket Units | On-demand service startup |

---

## Linux Capabilities

| Capability | Purpose |
|------------|---------|
| CAP_SYS_ADMIN | System administration |
| CAP_NET_ADMIN | Network management |
| CAP_NET_RAW | Raw sockets |
| CAP_SETUID | Change UID |
| CAP_SETGID | Change GID |
| CAP_SYS_PTRACE | Debug other processes |
| CAP_SYS_MODULE | Load kernel modules |
| CAP_DAC_OVERRIDE | Bypass file permissions |

---

## Linux Namespaces

| Namespace | Isolates |
|------------|----------|
| PID | Processes |
| Mount | Filesystems |
| Network | Network stack |
| IPC | IPC objects |
| UTS | Hostname |
| User | UID/GID mapping |
| Cgroup | Resource hierarchy |
| Time | System clocks |

---

## cgroups

| Resource | Controlled By |
|----------|---------------|
| CPU | CPU quotas |
| Memory | RAM limits |
| Disk I/O | Read/Write limits |
| Devices | Hardware access |
| Processes | PID limits |
| Swap | Swap usage |

---

## Linux Security Modules (LSM)

| Module | Purpose |
|---------|---------|
| SELinux | Label-based MAC |
| AppArmor | Profile-based MAC |
| Yama | Process restrictions |
| Smack | Mandatory Access Control |
| TOMOYO | Path-based MAC |
| Landlock | User-space sandboxing |
| BPF LSM | eBPF security policies |

---

## Common Security Boundaries

| Boundary | Purpose |
|----------|---------|
| User Space ↔ Kernel Space | Privilege separation |
| Process ↔ Process | Memory isolation |
| User ↔ Root | Privilege enforcement |
| Namespace ↔ Host | Container isolation |
| Service ↔ System | Least privilege |

---

## Common Privilege Escalation Targets

| Target | Risk |
|--------|------|
| SUID binaries | Root execution |
| Capabilities | Excessive privileges |
| Writable services | Code execution |
| Kernel vulnerabilities | Root compromise |
| Misconfigured systemd services | Persistence |
| Weak file permissions | Privilege escalation |

---

## Container Security Stack

| Component | Function |
|-----------|----------|
| Namespaces | Isolation |
| cgroups | Resource limits |
| Capabilities | Fine-grained privileges |
| LSM | Mandatory Access Control |
| seccomp | System call filtering |

---

## Important Directories

| Directory | Purpose |
|-----------|---------|
| /proc | Runtime process information |
| /sys | Kernel & hardware information |
| /etc | System configuration |
| /boot | Boot files |
| /dev | Device files |
| /var/log | System logs |
| /run | Runtime state |
| /lib/modules | Kernel modules |

---

## Offensive Mindset

1. Enumerate the system
2. Identify privilege boundaries
3. Analyze running services
4. Inspect processes and memory
5. Review capabilities
6. Check namespaces and cgroups
7. Examine kernel version
8. Look for misconfigurations
9. Identify persistence opportunities
10. Chain weaknesses

---

## Defensive Mindset

1. Apply least privilege
2. Minimize capabilities
3. Enable SELinux/AppArmor
4. Secure systemd services
5. Harden kernel configuration
6. Restrict namespaces
7. Configure cgroups
8. Keep kernel updated
9. Monitor processes and services
10. Audit logs regularly

---

## Key Insight

> Linux security is built on multiple layers—not a single control. Processes, memory, capabilities, namespaces, cgroups, systemd, and Linux Security Modules work together to enforce isolation, privilege separation, and resource control. Most successful attacks exploit weaknesses at the boundaries between these layers rather than flaws in any single component.
