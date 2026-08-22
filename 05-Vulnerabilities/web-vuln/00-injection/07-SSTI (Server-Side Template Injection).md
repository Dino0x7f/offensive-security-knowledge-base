# Server-Side Template Injection (SSTI)

## Overview

Server-Side Template Injection (SSTI) is a vulnerability that occurs when user-controlled input is embedded into a server-side template and interpreted as template code rather than plain text.

Modern web frameworks use template engines to generate dynamic HTML pages. If an application renders untrusted input directly within a template, attackers may execute template expressions, access internal objects, leak sensitive information, or even achieve Remote Code Execution (RCE).

Unlike Cross-Site Scripting (XSS), which executes code inside the victim's browser, SSTI executes on the **server**, making it significantly more dangerous.

---

# What is a Template Engine?

A template engine combines static templates with dynamic data to generate content.

Example:

Template:

```html
Hello {{ username }}
```

Application Data:

```
username = Alice
```

Rendered Output:

```html
Hello Alice
```

The template engine replaces placeholders before sending the response to the client.

---

# How SSTI Works

Typical flow:

```
User Input

↓

Application

↓

Template Engine

↓

Template Rendering

↓

Response
```

If user input becomes part of the template itself instead of being treated as data, the template engine may evaluate attacker-controlled expressions.

---

## Example

Safe template:

```html
Hello {{ username }}
```

Unsafe behavior:

```text
Hello {{ user_input }}
```

If the application evaluates user input as template code rather than escaping it, attacker-controlled expressions may execute on the server.

---

# Root Cause

SSTI occurs because applications treat untrusted input as executable template syntax.

Common causes include:

- Dynamic template generation
- Rendering user input directly
- Unsafe template compilation
- Custom template construction
- Missing output encoding
- Misconfigured template engines

---

# Attack Surface

SSTI commonly appears in:

- Search pages
- Email templates
- Report generation
- Administrative dashboards
- Notification systems
- CMS platforms
- PDF generation
- Dynamic HTML rendering

---

# Common Template Engines

Different languages use different template engines.

### Python

- Jinja2
- Tornado
- Mako

### Java

- FreeMarker
- Velocity
- Thymeleaf

### PHP

- Twig
- Smarty

### Node.js

- Handlebars
- Pug
- EJS
- Nunjucks

### Ruby

- ERB
- Slim
- Haml

Each template engine has different syntax and security characteristics.

---

# Types of SSTI

## Information Disclosure

Attackers retrieve:

- Configuration
- Environment variables
- File paths
- Framework objects
- Debug information

---

## Local File Access

Some template engines allow access to server-side files.

---

## Server-Side Code Execution

Certain template engines expose language features capable of executing code.

This often results in Remote Code Execution.

---

## Sandbox Escape

Some engines implement security sandboxes.

Attackers may abuse internal objects to escape these restrictions.

---

# Potential Impact

Successful SSTI may allow attackers to:

- Read sensitive information
- Access server-side objects
- Execute arbitrary code
- Read local files
- Access application secrets
- Retrieve credentials
- Execute operating system commands
- Fully compromise the server

The impact depends on the capabilities of the template engine.

---

# Common Indicators

Possible indicators include:

- Template evaluation errors
- Unexpected server-side calculations
- Framework-specific exceptions
- Exposure of internal objects
- Debug information leakage
- Abnormal template rendering

---

# Mitigation

Recommended defenses include:

- Never compile user-controlled templates
- Treat user input as data only
- Escape output correctly
- Use secure template APIs
- Enable template sandboxing
- Restrict template capabilities
- Keep template engines updated
- Perform security testing during development

Applications should never evaluate untrusted input as template code.

---

# Detection Methods

Security professionals identify SSTI through:

- Manual testing
- Source code review
- Template syntax analysis
- Dynamic security testing
- Fuzzing
- Automated security scanners

Different template engines require different testing approaches.

---

# SSTI vs XSS

| SSTI | XSS |
|------|-----|
| Executes on the server | Executes in the browser |
| Targets template engines | Targets client-side JavaScript |
| May lead to RCE | Usually affects browser security |
| Server compromise possible | Client compromise only |

Although both involve injected code, they execute in completely different environments.

---

# Relationship to Other Vulnerabilities

SSTI often serves as a bridge to more severe attacks.

```
SSTI

↓

Information Disclosure

↓

Server-Side Code Execution

↓

Command Execution

↓

Remote Code Execution

↓

Complete Server Compromise
```

---

# Real-World Examples

SSTI vulnerabilities have affected:

- Content Management Systems
- Administrative Portals
- Report Generators
- Email Services
- Cloud Platforms
- Enterprise Web Applications

Misconfigured template engines have repeatedly resulted in critical Remote Code Execution vulnerabilities.

---

# Importance in Offensive Security

Understanding SSTI enables penetration testers to:

- Assess server-side rendering security
- Evaluate template engine configurations
- Identify unsafe rendering logic
- Test sandbox implementations
- Demonstrate server-side impact
- Recommend secure template handling practices

---

> **Key Insight:** Server-Side Template Injection occurs when applications mistake user input for template code. Because template engines execute on the server, successful SSTI may escalate from simple information disclosure to full Remote Code Execution and complete server compromise.