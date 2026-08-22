# CSV Injection (Formula Injection)

## Overview

CSV Injection, also known as **Formula Injection**, is a vulnerability that occurs when an application exports untrusted user-controlled data into a CSV (Comma-Separated Values) file without neutralizing spreadsheet formulas.

When the exported CSV file is opened in spreadsheet software such as Microsoft Excel, LibreOffice Calc, or Google Sheets, cells beginning with specific formula characters are interpreted as executable formulas rather than plain text.

This may allow attackers to execute spreadsheet functions, manipulate data, exfiltrate information, or perform client-side attacks against users who open the exported file.

Unlike Server-Side Template Injection or Command Injection, CSV Injection executes **on the victim's spreadsheet application**, not on the web server.

---

# What is CSV?

CSV (Comma-Separated Values) is a plain text format used to exchange tabular data.

Example:

```csv
Name,Email,Role
Alice,alice@example.com,User
Bob,bob@example.com,Admin
```

Many applications allow users to export reports, logs, contacts, invoices, and audit records as CSV files.

---

# How CSV Injection Works

Typical flow:

```
User Input

↓

Application

↓

CSV Export

↓

Victim Opens File

↓

Spreadsheet Evaluates Formula
```

If user input begins with a spreadsheet formula character, the spreadsheet interprets it as executable content.

---

## Example

Application exports:

```csv
Username
Alice
Bob
```

If an attacker submits specially crafted input beginning with a formula indicator, the exported CSV may contain executable spreadsheet formulas instead of plain text.

---

# Root Cause

CSV Injection occurs because applications export user-controlled data without preventing spreadsheet software from interpreting it as a formula.

Common causes include:

- Unsanitized CSV exports
- Trusting user input
- Missing output encoding
- Lack of spreadsheet-specific protections
- Insecure reporting features

---

# Attack Surface

CSV Injection commonly appears in:

- User exports
- Contact exports
- Audit reports
- Employee reports
- Customer databases
- Ticketing systems
- CRM exports
- Financial reports
- Administrative dashboards

---

# Common Spreadsheet Applications

CSV Injection affects software that evaluates formulas automatically.

Examples include:

- Microsoft Excel
- LibreOffice Calc
- OpenOffice Calc
- Google Sheets
- Apple Numbers

Different applications support different functions and behaviors.

---

# Formula Prefix Characters

Spreadsheet applications typically interpret cells beginning with certain characters as formulas.

Common examples include:

| Character | Interpretation |
|-----------|----------------|
| `=` | Formula |
| `+` | Formula |
| `-` | Formula |
| `@` | Formula (modern Excel) |

Cells beginning with these characters should be considered potentially executable.

---

# Types of CSV Injection

## Formula Execution

Spreadsheet formulas execute automatically when the file is opened.

---

## Data Exfiltration

Spreadsheet functions may send information to external systems.

---

## User Interaction Abuse

Certain formulas require the victim to click links or approve prompts.

---

## Local System Interaction

Some spreadsheet applications provide functions capable of interacting with the local operating system, depending on configuration and security settings.

---

# Potential Impact

Successful exploitation may allow attackers to:

- Execute spreadsheet formulas
- Exfiltrate spreadsheet data
- Modify displayed information
- Mislead users
- Trigger external network requests
- Abuse spreadsheet features
- Support phishing attacks
- Target administrators reviewing exported reports

The impact depends on the spreadsheet application and its security configuration.

---

# Common Indicators

Possible indicators include:

- Unexpected spreadsheet formulas
- External network requests after opening CSV files
- Spreadsheet warnings
- Modified cell contents
- Formula execution prompts
- Unusual spreadsheet behavior

---

# Mitigation

Recommended defenses include:

- Escape formula characters
- Prefix dangerous values with a single quote (`'`)
- Validate exported data
- Encode user-controlled values
- Warn users before opening exported files
- Use secure export libraries
- Perform security testing of export functionality

Applications should treat CSV exports as executable content rather than plain text.

---

# Detection Methods

Security professionals identify CSV Injection through:

- Manual testing
- CSV export analysis
- Spreadsheet inspection
- Source code review
- Dynamic security testing
- Automated vulnerability scanners

Testing should include opening exported files in supported spreadsheet applications.

---

# CSV Injection vs XSS

| CSV Injection | XSS |
|---------------|-----|
| Executes in spreadsheet software | Executes in a web browser |
| Targets exported files | Targets web pages |
| Client-side execution | Client-side execution |
| Spreadsheet formula evaluation | JavaScript execution |

Both vulnerabilities execute on the client, but in different environments.

---

# Relationship to Other Vulnerabilities

CSV Injection is commonly part of administrative attack chains.

```
Stored User Input

↓

CSV Export

↓

Administrator Downloads Report

↓

Spreadsheet Opens File

↓

Formula Execution

↓

Client-Side Impact
```

It is particularly effective against privileged users who routinely export application data.

---

# Real-World Examples

CSV Injection has been identified in:

- Customer Relationship Management (CRM) systems
- Helpdesk platforms
- Human Resources applications
- Financial software
- Bug tracking systems
- Administrative reporting tools
- Cloud management platforms

Many vulnerabilities have resulted from applications assuming CSV files contain only passive data.

---

# Importance in Offensive Security

Understanding CSV Injection enables penetration testers to:

- Assess export functionality
- Evaluate reporting systems
- Test administrative workflows
- Identify client-side attack opportunities
- Demonstrate risks to privileged users
- Recommend secure CSV generation practices

---

> **Key Insight:** CSV Injection exploits trust in spreadsheet software rather than the web application itself. When applications export untrusted input without neutralizing spreadsheet formulas, attackers can transform seemingly harmless reports into client-side attack vectors targeting users who open the exported files.