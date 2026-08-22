# CRLF Injection

## Overview

CRLF Injection (Carriage Return Line Feed Injection) is a vulnerability that occurs when an application includes untrusted user input in HTTP headers, log entries, or protocol messages without properly sanitizing carriage return (`CR`) and line feed (`LF`) characters.

By injecting these control characters, attackers can terminate the current line and create new headers, modify protocol messages, poison log files, or enable secondary attacks such as HTTP Response Splitting.

CRLF Injection exploits the way text-based protocols interpret line boundaries rather than flaws in application logic.

---

# What are CR and LF?

HTTP and many other Internet protocols use two special control characters to terminate lines.

| Character | Name | ASCII |
|-----------|------|-------|
| `CR` | Carriage Return | 0x0D |
| `LF` | Line Feed | 0x0A |

Combined:

```
CR + LF

↓

End of Header Line
```

In HTTP, headers are separated using CRLF sequences.

---

# How CRLF Injection Works

Typical flow:

```
User Input

↓

Application

↓

HTTP Header

↓

Web Server

↓

Client
```

If an application inserts user-controlled input directly into an HTTP header, injected CRLF characters may terminate the current header and begin a new one.

---

## Example

Application generates:

```
Location: /home?user=alice
```

If the application accepts unfiltered CRLF characters, an attacker may alter the structure of the HTTP response by injecting additional header lines.

---

# Root Cause

CRLF Injection occurs because applications fail to validate or encode control characters before embedding user input into protocol messages.

Common causes include:

- Dynamic HTTP header generation
- Unsafe redirects
- Log generation
- Cookie creation
- Email header generation
- Proxy header manipulation
- Response construction

---

# Attack Surface

CRLF Injection commonly appears in:

- Redirect parameters
- HTTP response headers
- Set-Cookie headers
- Location headers
- Custom HTTP headers
- Logging systems
- Email generation
- Reverse proxies

---

# Common Targets

Applications that dynamically generate:

- HTTP Responses
- Cookies
- Redirects
- Web Server Logs
- Email Messages
- API Responses
- Reverse Proxy Headers

---

# Types of CRLF Injection

## HTTP Header Injection

Attackers inject additional HTTP headers.

Possible consequences include:

- Cookie manipulation
- Cache manipulation
- Security header modification

---

## HTTP Response Splitting

Injected CRLF sequences split one HTTP response into multiple responses.

This may enable:

- Cache Poisoning
- Cross-Site Scripting
- Content Injection

---

## Log Injection

Attackers inject fake log entries.

Possible goals include:

- Hiding malicious activity
- Misleading investigators
- Corrupting audit trails

---

## Email Header Injection

Applications generating emails may allow attackers to inject additional email headers.

Possible consequences include:

- Spam
- Recipient manipulation
- Email spoofing

---

# Potential Impact

Successful exploitation may allow attackers to:

- Inject HTTP headers
- Manipulate cookies
- Poison caches
- Forge log entries
- Alter redirects
- Bypass security controls
- Enable Cross-Site Scripting
- Support phishing attacks

The impact depends on where CRLF characters are processed.

---

# Common Indicators

Possible indicators include:

- Unexpected HTTP headers
- Duplicate responses
- Corrupted log files
- Unexpected redirects
- Invalid cookies
- Unusual proxy behavior

---

# Mitigation

Recommended defenses include:

- Reject CR (`\r`) characters
- Reject LF (`\n`) characters
- Encode user-controlled data
- Validate header values
- Use framework-provided HTTP APIs
- Avoid manual header construction
- Validate redirect targets

Applications should never allow user input to control protocol delimiters.

---

# Detection Methods

Security professionals identify CRLF Injection through:

- Manual testing
- HTTP header analysis
- Source code review
- Dynamic security testing
- Proxy inspection
- Automated vulnerability scanners

---

# CRLF Injection vs HTTP Response Splitting

| CRLF Injection | HTTP Response Splitting |
|---------------|-------------------------|
| Injects CRLF characters | Uses CRLF Injection to split responses |
| Primitive vulnerability | Exploitation technique |
| Alters protocol structure | Produces multiple HTTP responses |

HTTP Response Splitting is one of the most common consequences of CRLF Injection.

---

# Relationship to Other Vulnerabilities

CRLF Injection frequently enables additional attacks.

```
CRLF Injection

↓

HTTP Header Injection

↓

Response Splitting

↓

Cache Poisoning

↓

Cross-Site Scripting

↓

User Compromise
```

It often acts as a stepping stone rather than the final objective.

---

# Real-World Examples

CRLF Injection has been identified in:

- Web frameworks
- Reverse proxies
- Load balancers
- Redirect handlers
- Logging systems
- Email applications
- Web servers

Although modern frameworks sanitize most control characters by default, custom implementations remain vulnerable.

---

# Importance in Offensive Security

Understanding CRLF Injection enables penetration testers to:

- Assess HTTP response handling
- Evaluate header generation
- Test logging mechanisms
- Identify response splitting opportunities
- Analyze cache poisoning risks
- Recommend secure protocol handling

---

> **Key Insight:** CRLF Injection exploits trust in protocol formatting rather than application logic. By injecting line-terminating characters into HTTP headers or other text-based protocols, attackers can alter protocol structure, inject new headers, poison logs, and enable more complex attacks such as HTTP Response Splitting.