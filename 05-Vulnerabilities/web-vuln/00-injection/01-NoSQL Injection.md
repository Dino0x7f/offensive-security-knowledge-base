# NoSQL Injection

## Overview

NoSQL Injection (NoSQLi) is a vulnerability that occurs when untrusted user input is incorporated into NoSQL database queries without proper validation or parameterization. Unlike traditional SQL Injection, NoSQL Injection targets document-oriented, key-value, graph, or wide-column databases that use non-SQL query languages.

Modern web applications increasingly rely on NoSQL databases such as MongoDB, CouchDB, Cassandra, Elasticsearch, and Firebase. Improper handling of user-controlled data may allow attackers to manipulate database queries, bypass authentication, retrieve sensitive data, or modify stored information.

Although NoSQL databases do not use SQL syntax, they remain vulnerable whenever applications fail to distinguish user input from query logic.

---

# How NoSQL Injection Works

Typical application flow:

```
User Input

↓

Application

↓

NoSQL Query

↓

Database

↓

Response
```

Instead of manipulating SQL statements, attackers manipulate structured query objects.

---

## Example

Application:

```javascript
db.users.find({
    username: input.username,
    password: input.password
})
```

User input:

```json
{
    "username": "admin",
    "password": {
        "$ne": null
    }
}
```

Generated query:

```javascript
db.users.find({
    username: "admin",
    password: {
        "$ne": null
    }
})
```

The injected operator changes the query logic instead of supplying a normal value.

---

# Root Cause

NoSQL Injection occurs because applications trust user input when constructing database queries.

Common causes include:

- Direct insertion of JSON objects
- Unsafe object deserialization
- Missing input validation
- Dynamic query construction
- Improper ORM usage
- Failure to sanitize query operators

Unlike SQL Injection, the weakness often lies in allowing attackers to inject **query operators** rather than query syntax.

---

# Attack Surface

NoSQL Injection commonly appears in:

- Login forms
- Search functionality
- REST APIs
- GraphQL APIs
- JSON request bodies
- Filters
- Mobile applications
- Administrative dashboards

---

# Common Targets

NoSQL Injection primarily affects:

- MongoDB
- CouchDB
- Couchbase
- Elasticsearch
- Firebase
- Cassandra
- Redis (application-specific abuse)
- Neo4j (when queries are dynamically built)

---

# Common Injection Operators

Attackers frequently abuse database operators.

Examples include:

| Operator | Purpose |
|----------|---------|
| `$ne` | Not Equal |
| `$gt` | Greater Than |
| `$gte` | Greater Than or Equal |
| `$lt` | Less Than |
| `$lte` | Less Than or Equal |
| `$in` | Match Values |
| `$nin` | Exclude Values |
| `$or` | Logical OR |
| `$and` | Logical AND |
| `$regex` | Regular Expression |

These operators may alter application logic when accepted from user input.

---

# Types of NoSQL Injection

## Authentication Bypass

Attackers manipulate authentication queries to bypass credential verification.

---

## Operator Injection

Malicious database operators are injected into query objects.

---

## JavaScript Injection

Some NoSQL databases support server-side JavaScript execution.

Improper use of JavaScript evaluation may allow attackers to execute arbitrary expressions.

---

## Blind NoSQL Injection

Applications reveal little information directly.

Attackers infer results through:

- Response differences
- Boolean conditions
- Timing behavior

---

# Potential Impact

Successful exploitation may allow attackers to:

- Bypass authentication
- Read sensitive documents
- Modify stored records
- Delete collections
- Enumerate databases
- Access administrative accounts
- Execute server-side JavaScript (where supported)
- Escalate privileges

---

# Common Indicators

Possible indicators include:

- Authentication bypass
- Unexpected JSON parsing behavior
- Query errors
- Modified search results
- Abnormal application responses
- Unusual database operator usage

---

# Mitigation

Effective defenses include:

- Strict input validation
- Reject user-controlled query operators
- Use parameterized query builders
- Validate JSON schema
- Apply allow-list validation
- Use least-privileged database accounts
- Disable server-side JavaScript where unnecessary
- Implement proper access control

Applications should never allow users to directly control database query objects.

---

# Detection Methods

Security professionals identify NoSQL Injection through:

- Manual testing
- Source code review
- API security testing
- Dynamic application testing
- Fuzzing
- Automated scanners
- JSON parameter analysis

---

# SQL Injection vs NoSQL Injection

| SQL Injection | NoSQL Injection |
|--------------|-----------------|
| Targets relational databases | Targets NoSQL databases |
| Manipulates SQL statements | Manipulates query objects |
| Injects SQL syntax | Injects query operators |
| SQL parser executes payload | NoSQL query engine interprets payload |
| Examples: MySQL, PostgreSQL | Examples: MongoDB, CouchDB |

---

# Relationship to Other Vulnerabilities

NoSQL Injection may lead to:

```
NoSQL Injection

↓

Authentication Bypass

↓

Sensitive Data Access

↓

Privilege Escalation

↓

Application Compromise
```

---

# Real-World Examples

NoSQL Injection has been observed in:

- REST APIs
- Mobile backends
- Node.js applications
- Express.js applications
- MongoDB-powered authentication systems
- Cloud-native web services

The increasing adoption of NoSQL databases has expanded the relevance of this vulnerability.

---

# Importance in Offensive Security

Understanding NoSQL Injection enables penetration testers to:

- Assess modern web applications
- Evaluate API security
- Identify insecure query construction
- Test authentication mechanisms
- Assess document-oriented databases
- Recommend secure query practices

---

## Prerequisites

Before studying NoSQL Injection, you should understand:

- SQL Injection
- JSON Fundamentals
- NoSQL Databases
- REST APIs
- Vulnerability Fundamentals

---

## Next Step

Continue with:

**02-command-injection.md**

This chapter explores Command Injection, where attackers execute operating system commands by abusing insecure interaction between applications and the underlying operating system.

---

> **Key Insight:** NoSQL Injection is not about breaking SQL syntax it is about manipulating the logic of NoSQL query objects. Whenever applications allow user input to control database operators or query structure, attackers may alter query behavior and compromise the application's data.