# XML External Entity (XXE)

## Overview

XML External Entity (XXE) Injection is a vulnerability that occurs when an XML parser processes external entity declarations supplied by an attacker. By abusing XML entities, attackers can force the parser to access local files, internal network resources, or external systems.

Unlike XML Injection, which modifies the XML document itself, XXE exploits the **behavior of the XML parser**.

Because XML is widely used in SOAP services, SAML, Office document formats, and enterprise integrations, insecure XML parsers can expose highly sensitive information or become an entry point for server-side attacks.

---

# What is an XML Entity?

An XML entity is a placeholder that is replaced with a value during XML parsing.

Example:

```xml
<!ENTITY company "OpenAI">
```

Usage:

```xml
<name>&company;</name>
```

After parsing:

```xml
<name>OpenAI</name>
```

Entities can also reference **external resources**, which is where XXE vulnerabilities arise.

---

# How XXE Works

Typical processing flow:

```
XML Request

↓

Application

↓

XML Parser

↓

Entity Resolution

↓

Application Logic
```

If the parser allows external entities, an attacker can define malicious entity references.

---

## Example

Attacker-controlled XML:

```xml
<?xml version="1.0"?>

<!DOCTYPE data [
<!ENTITY secret SYSTEM "file:///etc/passwd">
]>

<data>
    <user>&secret;</user>
</data>
```

If external entities are enabled, the parser attempts to read the referenced file.

---

# Root Cause

XXE occurs when XML parsers allow external entity resolution without appropriate restrictions.

Common causes include:

- External entities enabled
- Default insecure parser configuration
- Legacy XML libraries
- Missing parser hardening
- Improper XML validation

---

# Attack Surface

XXE commonly appears in applications processing XML input.

Examples include:

- SOAP Web Services
- XML APIs
- File Upload Features
- Office Document Processing
- SVG Uploads
- SAML Authentication
- XML-RPC
- Enterprise Integration Systems

---

# Common Targets

Applications using:

- DOM Parsers
- SAX Parsers
- StAX Parsers
- SOAP Frameworks
- XML Configuration Files
- XML Import Features

---

# Types of XXE

## File Disclosure

Attackers force the parser to read local files.

Possible targets include:

- Configuration files
- Credentials
- API keys
- SSH keys
- Source code

---

## Server-Side Request Forgery (SSRF)

External entities reference internal network resources.

Example targets:

- Internal web services
- Metadata services
- Private APIs
- Localhost services

---

## Blind XXE

Applications do not directly return entity contents.

Attackers infer success through:

- External HTTP requests
- DNS lookups
- Timing differences
- Error messages

---

## Denial of Service

Malicious entities consume excessive parser resources.

Examples include:

- Recursive entities
- Large entity expansion
- Resource exhaustion

---

# Potential Impact

Successful XXE exploitation may allow attackers to:

- Read local files
- Access internal services
- Perform SSRF
- Leak credentials
- Discover application configuration
- Enumerate internal networks
- Trigger denial of service
- Support further attacks

---

# Common Indicators

Possible indicators include:

- Unexpected XML parsing errors
- Outbound server requests
- Internal resource access
- File disclosure
- Abnormal parser behavior
- DNS or HTTP callbacks

---

# Mitigation

Recommended defenses include:

- Disable external entity resolution
- Disable DTD processing
- Use secure XML parser configurations
- Keep XML libraries updated
- Validate XML against schemas
- Use less complex data formats where appropriate
- Restrict outbound network access

The most effective defense is to disable external entity processing unless explicitly required.

---

# Detection Methods

Security professionals identify XXE through:

- Manual testing
- XML request manipulation
- Source code review
- Dynamic application testing
- Blind XXE testing
- Automated security scanners

---

# XML Injection vs XXE

| XML Injection | XXE |
|--------------|-----|
| Modifies XML structure | Exploits XML parser behavior |
| Injects XML elements | Injects external entities |
| Targets document content | Targets entity resolution |
| Alters application logic | Reads resources or performs SSRF |

Although both involve XML, they exploit different weaknesses.

---

# Relationship to Other Vulnerabilities

XXE frequently leads to additional attacks.

```
XXE

↓

Local File Read

↓

Credential Disclosure

↓

Server-Side Request Forgery

↓

Internal Service Discovery

↓

Privilege Escalation
```

In many penetration tests, XXE serves as an initial foothold into internal infrastructure.

---

# Real-World Examples

XXE vulnerabilities have affected:

- SOAP APIs
- Banking platforms
- Enterprise middleware
- Document management systems
- Cloud applications
- Identity providers

Historically, XXE has been one of the most common XML-related vulnerabilities in enterprise software.

---

# Importance in Offensive Security

Understanding XXE enables penetration testers to:

- Assess XML parser security
- Test SOAP services
- Evaluate XML upload functionality
- Identify SSRF opportunities
- Discover sensitive server files
- Recommend secure parser configurations

---

> **Key Insight:** XXE is not caused by malicious XML itself, but by insecure XML parsers that resolve external entities. Proper parser configuration especially disabling external entity resolution and DTD processing is the primary defense against this class of vulnerabilities.