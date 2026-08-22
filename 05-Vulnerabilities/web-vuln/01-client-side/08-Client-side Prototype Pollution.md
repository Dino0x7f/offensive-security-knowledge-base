# Client-Side Prototype Pollution

## Overview

Client-Side Prototype Pollution is a JavaScript vulnerability that occurs when an application allows attacker-controlled input to modify the prototype of built-in JavaScript objects. Because JavaScript objects inherit properties from their prototypes, modifying a shared prototype affects every object derived from it within the application's execution context.

Unlike traditional Cross-Site Scripting (XSS), Prototype Pollution does not directly execute JavaScript. Instead, it corrupts the application's object model, potentially leading to application logic manipulation, privilege bypass, client-side denial of service, or even DOM XSS when polluted properties reach dangerous execution sinks.

Prototype Pollution has become increasingly important with the widespread use of JavaScript frameworks, object merging utilities, and client-side state management libraries.

---

# JavaScript Prototype Inheritance

JavaScript objects inherit properties through a prototype chain.

Simplified model:

```
Object

↓

Prototype

↓

Inherited Properties

↓

Application Objects
```

If the shared prototype is modified, every inheriting object may receive the injected property.

---

# How Client-Side Prototype Pollution Works

Typical execution flow:

```
Attacker Controls Input

↓

Unsafe Object Merge

↓

Prototype Modified

↓

Application Creates Objects

↓

Polluted Properties Used

↓

Unexpected Behavior
```

The vulnerability corrupts application behavior rather than immediately executing code.

---

# Root Cause

Prototype Pollution occurs because applications copy user-controlled properties into JavaScript objects without preventing modification of prototype-related properties.

Common causes include:

- Recursive object merging
- Deep object assignment
- Unsafe deserialization
- Dynamic object creation
- Query string parsers
- JSON parsing combined with unsafe merge logic

---

# Attack Surface

Client-Side Prototype Pollution commonly appears in:

- Single Page Applications (SPAs)
- State management libraries
- Configuration merging
- Client-side routing
- URL parameter parsing
- JSON configuration
- Form processing
- Third-party JavaScript libraries

Applications performing deep object manipulation are particularly susceptible.

---

# Common Pollution Sources

Attacker-controlled data may originate from:

- URL parameters
- URL fragments
- JSON payloads
- Form data
- Local Storage
- Session Storage
- Cookies
- Browser messaging
- API responses

Any user-controlled object used in merge operations may become an attack vector.

---

# Common Targets

Prototype Pollution typically affects:

- Plain JavaScript objects
- Configuration objects
- Framework options
- Component state
- User preferences
- Routing configurations
- Security settings

---

# Types of Prototype Pollution

## Property Injection

Shared prototype properties are modified.

---

## Configuration Pollution

Application configuration objects inherit attacker-controlled values.

---

## Logic Manipulation

Polluted properties alter application behavior.

---

## DOM XSS Chaining

Polluted values eventually reach dangerous DOM sinks, resulting in JavaScript execution.

---

## Client-Side Denial of Service

Prototype corruption causes application crashes or rendering failures.

---

# Potential Impact

Successful exploitation may allow attackers to:

- Modify application behavior
- Bypass client-side security checks
- Alter configuration values
- Trigger client-side denial of service
- Corrupt object inheritance
- Influence authorization logic
- Enable DOM XSS chains
- Assist privilege escalation within the application

The impact depends on how polluted properties are later consumed.

---

# Common Indicators

Possible indicators include:

- Unexpected object properties
- Application logic failures
- Browser console errors
- Configuration anomalies
- Unexplained DOM behavior
- Framework instability

Prototype Pollution often produces indirect symptoms rather than immediate code execution.

---

# Exploitation Requirements

Successful exploitation generally requires:

- User-controlled object properties
- Unsafe object merging
- Reachable prototype modification
- Polluted property consumption

Prototype Pollution alone is often insufficient; a vulnerable code path must later use the polluted property.

---

# Mitigation

Recommended defenses include:

- Reject prototype-related property names
- Use safe object merge libraries
- Validate object keys
- Prefer `Object.create(null)` where appropriate
- Freeze critical prototypes when feasible
- Keep JavaScript libraries updated
- Perform secure code reviews
- Avoid merging untrusted objects directly

Applications should explicitly prevent modification of shared prototypes.

---

# Detection Methods

Security professionals identify Prototype Pollution through:

- Manual testing
- JavaScript code review
- Source-to-sink analysis
- Dynamic browser testing
- Framework inspection
- Automated security scanners

Testing focuses on identifying unsafe object merge operations and prototype manipulation paths.

---

# Prototype Pollution vs DOM XSS

| Prototype Pollution | DOM XSS |
|----------------------|---------|
| Corrupts object inheritance | Executes JavaScript |
| Targets JavaScript prototypes | Targets DOM execution sinks |
| May not immediately execute code | Immediately executes attacker-controlled code |
| Often serves as a primitive | Often represents the final exploit |

Prototype Pollution frequently becomes dangerous when chained with DOM XSS.

---

# Relationship to Other Vulnerabilities

Prototype Pollution commonly enables more severe client-side attacks.

```
Prototype Pollution

↓

Configuration Manipulation

↓

Logic Corruption

↓

Unsafe DOM Sink

↓

DOM XSS

↓

Session Compromise
```

Many modern JavaScript vulnerabilities involve this progression.

---

# Real-World Examples

Client-Side Prototype Pollution has affected:

- JavaScript Frameworks
- State Management Libraries
- Query String Parsers
- Utility Libraries
- Component Frameworks
- Enterprise Single Page Applications

Numerous high-profile vulnerabilities have originated from insecure object merging functions in popular JavaScript libraries.

---

# Importance in Offensive Security

Understanding Client-Side Prototype Pollution enables penetration testers to:

- Assess JavaScript application security
- Analyze object merge operations
- Evaluate framework behavior
- Identify prototype manipulation opportunities
- Discover DOM XSS chains
- Recommend secure object handling practices

---

> **Key Insight:** Client-Side Prototype Pollution does not directly execute malicious code. Instead, it corrupts JavaScript's inheritance model by modifying shared prototypes, allowing attacker-controlled properties to influence application behavior. Its true impact emerges when polluted properties are later consumed by security-sensitive logic or dangerous DOM operations.