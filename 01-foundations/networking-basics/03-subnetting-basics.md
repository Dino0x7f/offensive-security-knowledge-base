# Subnetting (Security Perspective)

### Introduction

Subnetting is the process of dividing a larger IP network into smaller logical networks known as subnets. While subnetting was originally introduced to improve address utilization and network management, it has become one of the most important security mechanisms in modern enterprise environments.

From an offensive security perspective, subnetting defines network boundaries, influences visibility, and directly impacts an attacker's ability to move laterally after gaining initial access.

Understanding subnetting is therefore essential for both penetration testers and defenders.

---

### Why Subnetting Matters

Every enterprise network is built around multiple subnets.

Rather than placing every device within a single broadcast domain, organizations separate systems according to their purpose, sensitivity, and security requirements.

Proper subnetting helps security teams:

- Reduce the attack surface.
- Limit lateral movement.
- Separate critical infrastructure.
- Control communication between systems.
- Improve traffic monitoring.
- Enforce network segmentation policies.

For attackers, subnetting determines which systems are reachable and how difficult it is to expand access after the initial compromise.

---

### How Subnetting Works

A subnet represents a logical portion of an IP network.

For example, the network:

192.168.1.0/24

contains 256 IP addresses.

Instead of treating every host as part of one large network, it can be divided into multiple smaller segments.

#### Example:

192.168.1.0/26      User Workstations
192.168.1.64/26     Application Servers
192.168.1.128/26    Administrative Systems
192.168.1.192/26    Infrastructure Devices

Each subnet becomes an independent broadcast domain with its own routing and access control policies.

---

### Security Benefits

Network Isolation

Subnetting limits direct communication between groups of systems.

If one subnet is compromised, attackers cannot automatically access every other subnet.

This significantly reduces the impact of a successful intrusion.

---

### Controlled Communication

Traffic between subnets typically passes through routers or firewalls.

This enables organizations to enforce security controls such as:

- Access Control Lists (ACLs)
- Firewall Rules
- Network Segmentation Policies
- Zero Trust Network Access

Communication is explicitly allowed rather than implicitly trusted.

---

### Improved Visibility

Smaller network segments simplify monitoring.

Security teams can more easily:

- Detect abnormal traffic
- Identify unauthorized connections
- Investigate lateral movement
- Apply subnet-specific security policies

Segmentation improves both prevention and detection capabilities.

---

### Security Risks

Poor subnetting can significantly increase organizational risk.

Flat Networks

A flat network places every device within the same subnet.

Example:

192.168.1.0/24

All workstations, servers, printers, and administrative systems communicate freely.

Risks

- Easy host discovery
- Large attack surface
- Unrestricted lateral movement
- Increased ransomware impact
- Weak internal security boundaries

Flat networks remain common in small organizations and are frequently observed during internal penetration tests.

---

### Over-Permissive Routing

Subnetting alone does not provide security.

If routers or firewalls allow unrestricted communication between subnets, segmentation becomes ineffective.

Examples include:

- Any-to-any firewall rules
- Unrestricted SMB traffic
- Open RDP access
- Unfiltered management protocols

Attackers often exploit overly permissive routing to move across the enterprise.

---

### Attacker Perspective

After obtaining initial access, attackers seek to understand the internal network structure.

Typical objectives include:

- Enumerate available subnets.
- Identify routing paths.
- Locate Domain Controllers.
- Discover database servers.
- Identify management networks.
- Detect backup infrastructure.
- Find security monitoring systems.
- Build lateral movement paths.

Subnet discovery becomes a critical step during post-exploitation.

---

### Defensive Perspective

Security teams use subnetting to implement defense-in-depth.

Common strategies include:

- Separate user and server networks.
- Isolate Domain Controllers.
- Restrict administrative workstations.
- Protect backup infrastructure.
- Create dedicated management networks.
- Limit east-west traffic.
- Monitor communication between sensitive segments.

Effective segmentation slows attackers and increases opportunities for detection.

---

### Common Misconceptions

Several misconceptions exist regarding subnetting:

- Subnetting alone provides security.
- Internal networks are trusted by default.
- Smaller subnets automatically prevent attacks.
- Firewalls are unnecessary between internal segments.

In practice, subnetting must be combined with proper routing policies, firewall enforcement, authentication controls, and continuous monitoring.

---

### Real-World Example

Consider an organization with the following design:

Users -------------------- 192.168.10.0/24
Servers ------------------ 192.168.20.0/24
Domain Controllers ------- 192.168.30.0/24
Management Network ------- 192.168.40.0/24

If a workstation within the Users subnet is compromised, properly configured firewalls can prevent direct communication with Domain Controllers and management systems.

Without segmentation, the attacker may enumerate and access critical infrastructure immediately after gaining initial access.

---

### Key Takeaways

- Subnetting divides networks into logical security boundaries.
- Network segmentation reduces attack surface and limits lateral movement.
- Poor segmentation enables rapid compromise of enterprise environments.
- Effective subnetting combines logical separation with strict routing and firewall policies.
- Understanding subnet layouts is essential for realistic attack path analysis during penetration testing.
