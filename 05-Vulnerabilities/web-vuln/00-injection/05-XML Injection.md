# XML Injection

## Overview

XML Injection is a vulnerability that occurs when untrusted user input is inserted into an XML document without proper validation or encoding, allowing attackers to modify the XML structure, inject new elements or attributes, or alter the application's intended behavior.

Unlike XPath Injection, which manipulates **XPath queries**, XML Injection manipulates the **XML document itself**.

Applications that exchange, store, or process XML data are particularly susceptible if user input is directly embedded into XML documents.

---

# What is XML?

XML (eXtensible Markup Language) is a markup language used to represent structured data.

Example:

```xml
<user>
    <username>alice</username>
    <role>user</role>
</user>
```

XML is commonly used in:

- SOAP Web Services
- Configuration Files
- SAML Authentication
- XML APIs
- Office Documents
- Enterprise Integration Systems

---

# How XML Injection Works

Typical application flow:

```
User Input

↓

Application

↓

XML Document

↓

XML Parser

↓

Application Logic
```

If user input becomes part of an XML document without proper encoding, attackers may inject additional XML elements or alter existing ones.

---

## Example

Application creates:

```xml
<user>
    <name>John</name>
</user>
```

User input:

```text
John</name><admin>true</admin><name>
```

Generated XML:

```xml
<user>
    <name>John</name>
    <admin>true</admin>
    <name></name>
</user>
```

The injected XML changes the document's structure.

---

# Root Cause

XML Injection occurs when applications fail to distinguish between XML data and XML markup.

Common causes include:

- Dynamic XML generation
- Missing XML encoding
- Unsafe string concatenation
- Improper XML serialization
- Insecure XML builders
- Lack of schema validation

---

# Attack Surface

XML Injection commonly appears in:

- SOAP Requests
- XML APIs
- SAML Assertions
- XML Configuration Files
- XML Import Features
- Enterprise Middleware
- XML-Based Authentication Systems

---

# Common Targets

Applications using:

- SOAP
- SAML
- XML-RPC
- Configuration Management Systems
- Enterprise Integration Platforms
- XML Databases
- Identity Providers

---

# Types of XML Injection

## Element Injection

Attackers insert additional XML elements.

---

## Attribute Injection

Attackers inject new XML attributes.

---

## XML Structure Manipulation

Attackers modify the hierarchy of the XML document.

---

## Authentication Manipulation

Injected XML changes authentication or authorization data.

---

## Business Logic Manipulation

Injected elements alter how the application processes XML data.

---

# Potential Impact

Successful XML Injection may allow attackers to:

- Modify XML documents
- Inject unauthorized data
- Manipulate application logic
- Bypass business rules
- Corrupt application state
- Escalate privileges
- Support additional XML-based attacks

The impact depends on how the application processes the injected XML.

---

# XML Injection vs XPath Injection

| XML Injection | XPath Injection |
|--------------|-----------------|
| Modifies XML documents | Modifies XPath queries |
| Targets XML structure | Targets query logic |
| Injects elements or attributes | Injects XPath operators |
| Affects parsing | Affects querying |

Although related, they target different components of XML processing.

---

# Common Indicators

Possible indicators include:

- XML parsing errors
- Unexpected XML nodes
- Modified application behavior
- Authentication anomalies
- Invalid XML documents
- Unexpected server responses

---

# Mitigation

Recommended defenses include:

- Proper XML encoding
- Safe XML serialization libraries
- XML Schema (XSD) validation
- Strict input validation
- Avoid manual XML construction
- Secure XML parsers
- Least-privileged application design

Applications should never concatenate user input directly into XML documents.

---

# Detection Methods

Security professionals identify XML Injection through:

- Manual testing
- Source code review
- XML request analysis
- Dynamic application testing
- Fuzzing
- Automated security scanners

---

# Relationship to Other XML Attacks

XML Injection belongs to a broader family of XML-related vulnerabilities.

```
XML Security

├── XML Injection
├── XPath Injection
├── XXE (XML External Entity)
├── XSLT Injection
└── SOAP Injection
```

Each targets a different stage of XML processing.

---

# Real-World Examples

XML Injection has been identified in:

- SOAP Web Services
- Enterprise Middleware
- Identity Management Systems
- XML Configuration Interfaces
- Banking Applications
- Government Information Systems

Although modern REST APIs have reduced XML usage, many enterprise environments still depend heavily on XML technologies.

---

# Importance in Offensive Security

Understanding XML Injection enables penetration testers to:

- Assess XML-based applications
- Evaluate SOAP services
- Test SAML implementations
- Identify insecure XML generation
- Analyze XML parsing logic
- Recommend secure XML handling practices

---

## Prerequisites

Before studying XML Injection, you should understand:

- XML Fundamentals
- Web Fundamentals
- SOAP Basics
- XPath Injection
- Vulnerability Fundamentals

---

> **Key Insight:** XML Injection occurs when applications treat user input as XML markup rather than plain data. By injecting or modifying XML elements and attributes, attackers can alter document structure, manipulate application logic, and compromise XML-based systems.