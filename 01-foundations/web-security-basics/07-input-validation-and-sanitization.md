# Input Validation and Sanitization (Security Perspective)

## Overview

Input validation is the process of verifying that data received from users, APIs, or external systems conforms to expected rules before it is processed.

Sanitization is the process of safely transforming input so it can be used without introducing security risks.

From a security perspective:

> Every piece of external input is attacker-controlled until validated and handled securely.

---

# Why Input Validation Matters

Nearly every web application accepts external input through:

- Forms
- URL parameters
- JSON requests
- HTTP headers
- Cookies
- File uploads
- API requests

Improper handling of this input can lead to:

- Injection attacks
- Cross-Site Scripting (XSS)
- Path Traversal
- Business Logic abuse
- Authentication bypass
- Remote Code Execution

Input validation is one of the first defensive controls in secure application design.

---

# Validation vs Sanitization

## Validation

Validation determines whether input is acceptable.

Questions include:

- Is the input expected?
- Is the format correct?
- Is the length acceptable?
- Is the data type valid?

Invalid input should be rejected.

---

## Sanitization

Sanitization transforms valid input into a safe representation before processing or displaying it.

Examples include:

- Escaping HTML characters
- Encoding output
- Removing unsupported characters
- Normalizing input format

Sanitization reduces risk but does not replace validation.

---

# Validation Strategies

## 1. Allowlist Validation (Recommended)

Accept only predefined valid input.

Examples:

- Numbers only
- Email format
- UUID format
- ISO date format
- Limited character sets

### Advantages

- Predictable behavior
- Difficult to bypass
- Strong security model

---

## 2. Blocklist Validation (Weak)

Reject known malicious patterns.

Examples:

- `<script>`
- `' OR 1=1`
- `../`

### Problems

- Impossible to block every payload
- Easy to bypass using encoding or new techniques
- Requires constant updates

---

# Server-Side vs Client-Side Validation

## Client-Side Validation

Runs inside the browser.

Purpose:

- Improve user experience
- Reduce invalid requests

Limitations:

- Can be disabled
- Can be modified
- Cannot be trusted

---

## Server-Side Validation (Mandatory)

Runs on the server before processing.

Purpose:

- Enforce security
- Validate every request
- Protect backend resources

Server-side validation should always be considered the source of truth.

---

# Common Validation Targets

Applications should validate:

- Length
- Data type
- Character set
- Numeric ranges
- File extensions
- MIME types
- Request size
- JSON structure
- Parameter count
- Expected formats

---

# Output Encoding

Even valid input can become dangerous when displayed.

Output should be encoded according to its context:

- HTML encoding
- JavaScript encoding
- URL encoding
- CSS encoding

Proper output encoding is one of the primary defenses against XSS.

---

# Common Validation Mistakes

## Trusting Client-Side Validation

JavaScript restrictions can always be bypassed.

---

## Missing Server-Side Validation

Allows malicious requests to reach application logic.

---

## Using Blocklists Only

Attackers constantly develop new payloads that bypass filters.

---

## Improper Escaping

Failing to encode output correctly can introduce XSS vulnerabilities.

---

## Trusting HTTP Headers

Headers such as:

- User-Agent
- Referer
- Host
- X-Forwarded-For

are completely attacker-controlled.

---

## Trusting File Extensions

A filename alone does not guarantee file safety.

File content, MIME type, and processing logic must also be validated.

---

# Security Best Practices

- Validate every input
- Validate on the server
- Prefer allowlists over blocklists
- Apply strict type checking
- Enforce reasonable length limits
- Encode output based on context
- Reject unexpected input
- Never concatenate untrusted input into commands or queries

---

# Attacker Perspective

Attackers attempt to:

- Bypass validation rules
- Abuse parser inconsistencies
- Exploit encoding differences
- Inject malicious payloads
- Manipulate hidden parameters
- Abuse file upload validation
- Trigger unexpected application behavior

Rather than asking:

*"Can I send invalid input?"*

Attackers ask:

*"Can I make the application interpret my input differently than the developer intended?"*

---

# Security Mindset

Validation should be viewed as enforcing a contract between the client and the server.

Every deviation from expected input should be treated as potentially malicious until verified.

---

# Key Takeaways

- Every external input is untrusted.
- Validation determines whether input is acceptable.
- Sanitization makes acceptable input safe to process.
- Client-side validation improves usability, not security.
- Server-side validation is mandatory.
- Output encoding protects users from injected content.
- Allowlists provide stronger security than blocklists.

---

# Key Insight

> Secure applications do not assume input is safe they verify it, constrain it, and handle it according to context before allowing it to influence application behavior.
