# Web Security Basics

## Overview

This section introduces the fundamental concepts of web application security from an offensive security perspective.

The goal is to understand how web applications work, where trust boundaries exist, and how attackers exploit vulnerabilities in application logic, authentication, authorization, and user input.

---

## Contents

### Web Security Overview
Introduction to web applications, attack surfaces, and core security principles.

### HTTP & HTTPS
Understanding web communication, TLS, and transport security.

### HTTP Headers and Methods
HTTP methods, request headers, response headers, and their security implications.

### Cookies and Sessions
Session management, authentication cookies, and common session attacks.

### Authentication vs Authorization
Identity verification, access control, and common implementation flaws.

### OWASP Top 10
Overview of the most critical web application security risks.

### Common Web Vulnerabilities
Injection, XSS, CSRF, SSRF, IDOR, file upload vulnerabilities, and business logic flaws.

### Input Validation and Sanitization
Server-side validation, input handling, and secure data processing.

### Same-Origin Policy (SOP) and CORS
Browser security model, cross-origin requests, and common CORS misconfigurations.

### Web Architecture Basics
High-level architecture of modern web applications and trust boundaries.

### API Security Basics
REST APIs, GraphQL, authentication, authorization, and common API vulnerabilities.

### Web Security Cheatsheet
Quick reference covering the essential web security concepts and attack patterns.

---

## Learning Objectives

After completing this section, you should be able to:

- Understand how web applications communicate
- Explain HTTP and HTTPS security
- Understand sessions, cookies, and authentication
- Differentiate authentication from authorization
- Identify common web vulnerabilities
- Understand browser security mechanisms
- Analyze APIs from a security perspective
- Recognize common web misconfigurations

---

## Security Mindset

Web applications process untrusted input every second.

Successful attacks usually result from:
- Trusting user input
- Weak access control
- Poor session management
- Logic flaws
- Security misconfigurations

---

## Key Takeaway

> Web security is not about blocking requests, it is about controlling how untrusted input is processed, authenticated, authorized, and transformed into application behavior.
