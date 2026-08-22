# ldapsearch

## Overview

ldapsearch is a command-line LDAP client used to query Lightweight Directory Access Protocol (LDAP) directories. It allows security professionals to search, retrieve, and enumerate objects stored in directory services such as Microsoft Active Directory, OpenLDAP, and other LDAP-compliant identity management systems.

Unlike automated Active Directory frameworks, ldapsearch provides direct access to the LDAP database using customizable search filters, making it an essential tool for manual directory enumeration.

It is widely used during Active Directory assessments, Red Team operations, identity audits, and internal penetration testing.

---

# Core Objectives

ldapsearch is designed to answer questions such as:

- Which users exist?
- Which groups exist?
- What Organizational Units (OUs) are present?
- Which computers are joined to the domain?
- What attributes are associated with an object?
- What domain information is publicly accessible?
- Is anonymous LDAP access allowed?

Its primary objective is structured directory enumeration through LDAP queries.

---

# Architecture

Typical workflow:

```
Target LDAP Server

↓

LDAP Connection

↓

Authentication

↓

LDAP Search Filter

↓

Directory Objects

↓

Attribute Extraction

↓

Attack Surface Analysis
```

ldapsearch retrieves objects by querying the directory tree using LDAP filters.

---

# LDAP Directory Structure

LDAP stores information hierarchically.

Typical structure:

```
Domain

↓

Organizational Units (OU)

↓

Users

↓

Groups

↓

Computers

↓

Service Accounts

↓

Policies
```

Each object contains multiple attributes describing its identity and configuration.

---

# Core Capabilities

## User Enumeration

Retrieves:

- Domain users
- Service accounts
- Administrative accounts
- User attributes

---

## Group Enumeration

Enumerates:

- Security groups
- Distribution groups
- Built-in groups
- Administrative groups

---

## Computer Enumeration

Collects:

- Computer accounts
- Hostnames
- Operating systems
- Server information

---

## Organizational Unit Enumeration

Retrieves:

- Departmental structure
- Administrative hierarchy
- Object organization

---

## Attribute Enumeration

Common attributes include:

- sAMAccountName
- distinguishedName
- objectClass
- objectGUID
- memberOf
- description
- mail
- userPrincipalName
- servicePrincipalName (SPN)

---

## Domain Information

Collects:

- Domain naming context
- Domain controllers
- Forest information
- Functional level
- LDAP schema information

---

# LDAP Search Filters

ldapsearch uses filters to limit search results.

Common filters include:

- Users
- Groups
- Computers
- Service accounts
- Specific attributes
- Disabled accounts
- Administrative accounts

Efficient filtering significantly improves enumeration quality.

---

# Attack Surface

ldapsearch primarily targets:

- Active Directory
- OpenLDAP
- LDAP servers
- Domain Controllers
- Identity management systems

It communicates using:

- LDAP (389/TCP)
- LDAPS (636/TCP)

---

# Common Use Cases

ldapsearch is commonly used for:

- Active Directory enumeration
- LDAP auditing
- Identity reconnaissance
- Service account discovery
- SPN enumeration
- User discovery
- Group analysis
- Internal penetration testing

It provides precise access to directory information unavailable through SMB alone.

---

# Anonymous Enumeration

Depending on server configuration, ldapsearch may allow anonymous retrieval of:

- Domain information
- User objects
- Group objects
- Organizational Units
- Schema information

Modern Active Directory deployments usually restrict anonymous LDAP queries.

---

# Detection and Logging

ldapsearch activity may be detected through:

- Active Directory logs
- LDAP query logs
- SIEM platforms
- Domain Controller monitoring
- EDR solutions

Indicators include:

- LDAP bind requests
- Large directory searches
- High-volume object queries
- Attribute enumeration
- Anonymous bind attempts

Large-scale LDAP enumeration is commonly monitored in enterprise environments.

---

# Strengths

ldapsearch provides:

- Direct LDAP access
- Flexible search filters
- Precise attribute retrieval
- Lightweight operation
- Standards-based protocol support
- Active Directory compatibility
- OpenLDAP compatibility
- Fine-grained directory queries

Its flexibility makes it one of the most powerful manual LDAP enumeration tools.

---

# Limitations

ldapsearch cannot:

- Execute remote commands
- Exploit vulnerabilities
- Perform privilege escalation
- Replace BloodHound graph analysis
- Replace SMB enumeration

Its scope is limited to LDAP directory queries.

---

# Comparison with Other Enumeration Tools

| Tool | Primary Purpose |
|------|-----------------|
| ldapsearch | Manual LDAP enumeration |
| NetExec | Automated AD enumeration |
| BloodHound | Privilege relationship analysis |
| enum4linux | SMB enumeration |
| rpcclient | RPC enumeration |
| PowerView | PowerShell-based AD enumeration |

ldapsearch offers low-level LDAP access, whereas other tools automate higher-level workflows.

---

# Relationship to Other Enumeration Tools

ldapsearch commonly integrates with:

- NetExec
- BloodHound
- Kerbrute
- Impacket
- rpcclient
- enum4linux
- Nmap

LDAP data often complements SMB and Kerberos enumeration during Active Directory assessments.

---

# Role in Penetration Testing

Typical workflow:

```
Host Discovery

↓

LDAP Service Detected

↓

LDAP Bind

↓

User Enumeration

↓

Group Enumeration

↓

Computer Enumeration

↓

Service Account Discovery

↓

Attack Surface Analysis
```

The information gathered supports password attacks, Kerberoasting, privilege analysis, and Active Directory mapping.

---

# Common Commands

| Purpose | Example |
|----------|---------|
| Anonymous LDAP Query | `ldapsearch -x -H ldap://<target>` |
| Authenticated Bind | `ldapsearch -x -H ldap://<target> -D "<user>" -W` |
| Retrieve Naming Context | `ldapsearch -x -H ldap://<target> -s base namingcontexts` |
| Enumerate All Objects | `ldapsearch -x -H ldap://<target> -b "DC=example,DC=com"` |
| Enumerate Users | `ldapsearch -x -H ldap://<target> -b "DC=example,DC=com" "(objectClass=user)"` |
| Enumerate Groups | `ldapsearch -x -H ldap://<target> -b "DC=example,DC=com" "(objectClass=group)"` |
| Enumerate Computers | `ldapsearch -x -H ldap://<target> -b "DC=example,DC=com" "(objectClass=computer)"` |
| Enumerate Service Accounts (SPNs) | `ldapsearch -x -H ldap://<target> "(servicePrincipalName=*)"` |

> These commands represent the most common LDAP reconnaissance operations. Advanced LDAP filters and complex attribute queries should be documented separately in the LDAP or Active Directory Cheatsheet.

---

# Importance in Offensive Security

Understanding ldapsearch enables penetration testers to:

- Enumerate Active Directory
- Retrieve directory objects
- Discover users and groups
- Identify service accounts
- Analyze domain structure
- Support Kerberos-based attacks
- Map enterprise identities

ldapsearch remains an essential enumeration tool because it provides direct, standards-based access to LDAP directories, allowing precise retrieval of identity information that forms the foundation of many Active Directory attack paths.

---

> **Key Insight:** ldapsearch is a direct window into the directory service. Rather than relying on automated frameworks, it allows precise LDAP queries that reveal users, groups, computers, service accounts, and domain structure, making it indispensable for manual Active Directory reconnaissance.