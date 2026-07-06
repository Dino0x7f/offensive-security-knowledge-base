# Operating Systems Basics

## Overview

This section introduces the fundamental concepts of operating systems from an offensive security perspective.

The goal is to understand how Windows and Linux manage processes, memory, files, permissions, and system resources, and how attackers abuse these components during system compromise.

---

## Contents

### Operating Systems Overview
Introduction to operating systems, their architecture, and their role in modern security.

### Linux Fundamentals
Linux file system, users, permissions, services, networking, and common attack surfaces.

### Windows Fundamentals
Windows architecture, registry, services, Active Directory integration, and enterprise security concepts.

### File Systems
Storage structure, file permissions, sensitive files, and common security weaknesses.

### Users and Groups
Identity management, privilege models, and access control mechanisms.

### Permission Models
Linux permissions, Windows ACLs, SUID/SGID, and privilege-related security risks.

### Processes and Memory
Process execution, memory management, process injection, credential dumping, and memory-based attacks.

### Services and Daemons
Background services, exposed attack surfaces, service misconfigurations, and privilege escalation opportunities.

### System Logs
Linux and Windows logging systems, detection, forensics, and log manipulation.

### Networking in Operating Systems
Network interfaces, ports, sockets, firewalls, and operating system networking fundamentals.

### System Hardening Basics
Reducing attack surface through secure configuration, least privilege, and service hardening.

### Common Misconfigurations
Default credentials, weak permissions, exposed services, firewall mistakes, and insecure configurations.

### Pentester Perspective
How penetration testers analyze operating systems, identify weaknesses, and build attack paths.

### Operating Systems Cheatsheet
Quick reference covering essential operating system concepts and common security issues.

---

## Learning Objectives

After completing this section, you should be able to:

- Understand the architecture of Windows and Linux
- Explain how operating systems manage processes, memory, and files
- Understand permission and access control models
- Identify common operating system attack surfaces
- Recognize privilege escalation opportunities
- Analyze services and system configurations
- Understand system hardening principles
- Evaluate operating systems from a penetration testing perspective

---

## Security Mindset

Operating systems enforce trust boundaries through identities, permissions, processes, and services.

Most successful attacks result from:
- Weak permissions
- Misconfigured services
- Credential exposure
- Excessive privileges
- Poor system hardening

---

## Key Takeaway

> Operating systems are the foundation of every computing environment. Understanding how they manage execution, access, and trust is essential for identifying weaknesses, exploiting vulnerabilities, and defending modern systems.
