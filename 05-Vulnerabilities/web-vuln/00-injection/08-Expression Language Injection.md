# Expression Language Injection (EL Injection)

## Overview

Expression Language Injection (EL Injection) is a vulnerability that occurs when an application evaluates user-controlled input as an expression in a server-side Expression Language (EL) engine.

Instead of treating user input as plain text, the application interprets it as executable expressions capable of accessing objects, invoking methods, or interacting with the application runtime.

EL Injection is primarily associated with Java technologies such as **Java Expression Language (JSP EL)**, **Spring Expression Language (SpEL)**, **OGNL (Object Graph Navigation Language)**, **Unified EL**, and frameworks including **Spring**, **Apache Struts**, and **Jakarta EE**.

Successful exploitation may lead to information disclosure, authentication bypass, arbitrary method invocation, or even Remote Code Execution.

---

# What is an Expression Language?

Expression Languages allow applications to dynamically evaluate expressions at runtime.

Example:

```text
${user.name}
```

The expression engine evaluates the expression and returns the value.

Expression Languages commonly provide access to:

- Variables
- Objects
- Properties
- Methods
- Functions
- Application Context

---

# How EL Injection Works

Typical flow:

```
User Input

↓

Application

↓

Expression Engine

↓

Expression Evaluation

↓

Application Response
```

If user-controlled input is evaluated as an expression, the attacker may execute unintended operations.

---

## Example

Application expects:

```text
Hello ${username}
```

Instead of displaying user input as text, the application evaluates it as an expression.

If user input reaches the expression engine without validation, arbitrary expression evaluation becomes possible.

---

# Root Cause

EL Injection occurs because applications evaluate untrusted input as executable expressions.

Common causes include:

- Dynamic expression evaluation
- Unsafe template rendering
- User-controlled EL evaluation
- Framework misconfiguration
- Reflection-based object access
- Insecure expression parsers

---

# Attack Surface

Expression Language Injection commonly appears in:

- Java Web Applications
- Administrative Panels
- Template Rendering
- Search Interfaces
- Configuration Editors
- Report Generation
- Workflow Engines
- Expression-Based Rules

---

# Common Technologies

EL Injection is associated with:

### Java EL

- JSP Expression Language
- Jakarta EL
- Unified EL

### Spring

- Spring Expression Language (SpEL)

### Apache Struts

- OGNL (Object Graph Navigation Language)

### Other Engines

- MVEL
- JEXL
- Aviator
- Custom Expression Engines

---

# Types of EL Injection

## Information Disclosure

Attackers access:

- Environment variables
- Application configuration
- Internal objects
- Runtime information

---

## Method Invocation

Expressions invoke application methods unexpectedly.

---

## Property Access

Expressions access internal object properties.

---

## Authentication Bypass

Expression evaluation alters application logic.

---

## Remote Code Execution

Certain expression engines expose runtime functionality capable of executing arbitrary code.

This is the most severe outcome.

---

# Potential Impact

Successful EL Injection may allow attackers to:

- Read sensitive information
- Access internal application objects
- Invoke arbitrary methods
- Bypass security controls
- Modify application behavior
- Execute system commands
- Achieve Remote Code Execution

The impact depends on the capabilities of the expression engine.

---

# Common Indicators

Possible indicators include:

- Expression evaluation errors
- Unexpected server responses
- Framework-specific exceptions
- Runtime object disclosure
- Debug information exposure
- Abnormal application behavior

---

# Mitigation

Recommended defenses include:

- Never evaluate user-controlled expressions
- Disable unnecessary expression evaluation
- Validate input using allow-lists
- Escape user input before rendering
- Restrict accessible objects
- Enable framework security features
- Keep frameworks updated
- Perform secure code reviews

Applications should always treat external input as data rather than executable expressions.

---

# Detection Methods

Security professionals identify EL Injection through:

- Manual testing
- Source code review
- Framework analysis
- Dynamic security testing
- Expression fuzzing
- Automated vulnerability scanners

Different Java frameworks expose different attack vectors.

---

# EL Injection vs SSTI

| EL Injection | SSTI |
|--------------|------|
| Targets expression engines | Targets template engines |
| Executes expressions | Executes templates |
| Common in Java frameworks | Found in many programming languages |
| May invoke application objects | May execute template code |

Although related, EL Injection specifically targets expression evaluation mechanisms rather than template rendering engines.

---

# Relationship to Other Vulnerabilities

Expression Language Injection often escalates into more severe attacks.

```
EL Injection

↓

Object Access

↓

Method Invocation

↓

Information Disclosure

↓

Code Execution

↓

Remote Code Execution
```

Many critical Java framework vulnerabilities have followed this progression.

---

# Real-World Examples

EL Injection vulnerabilities have affected:

- Apache Struts
- Spring Framework
- Jakarta EE Applications
- Enterprise Java Portals
- Java-Based CMS Platforms
- Workflow Automation Systems

Several high-profile Remote Code Execution vulnerabilities originated from insecure expression evaluation.

---

# Importance in Offensive Security

Understanding EL Injection enables penetration testers to:

- Assess Java web applications
- Evaluate expression engine security
- Identify unsafe runtime evaluation
- Test enterprise frameworks
- Demonstrate server-side impact
- Recommend secure expression handling

---

> **Key Insight:** Expression Language Injection occurs when applications evaluate untrusted input as executable expressions rather than plain text. In modern Java frameworks, insecure expression evaluation can expose internal application objects, invoke arbitrary methods, and ultimately lead to complete server compromise.