# Task 1 - Vulnerability Assessment Report

**Future Interns - Cyber Security Internship | April 2026**

## Overview

This repository contains the vulnerability assessment report completed as part of Task 1 of the Future Interns Cyber Security Internship program. The assessment was conducted on DVWA (Damn Vulnerable Web Application), a deliberately vulnerable web application used for security testing and learning purposes.

## Target

| Field | Details |
|---|---|
| Application | DVWA (Damn Vulnerable Web Application) v2.4 |
| URL | http://localhost/dvwa |
| IP Address | 127.0.0.1 (local environment) |
| Assessment Type | Passive / Non-Destructive |
| Date | April 10, 2026 |

## Tools Used

| Tool | Version | Purpose |
|---|---|---|
| Nmap | 7.99 | Network port scanning and service detection |
| OWASP ZAP | 2.17.0 | Passive web vulnerability scanning via HTTP proxy |
| Browser DevTools | Firefox | Manual HTTP response header inspection |
| Kali Linux | 2024.x (VirtualBox VM) | Security assessment environment |

## Findings Summary

| # | Vulnerability | Risk Level | Priority |
|---|---|---|---|
| 1 | SQL Injection | HIGH | Immediate |
| 2 | Missing Anti-CSRF Tokens | MEDIUM | Short term |
| 3 | CSP Header Not Set | MEDIUM | Short term |
| 4 | Missing Anti-Clickjacking Header | MEDIUM | Short term |
| 5 | Cookie without SameSite Attribute | MEDIUM | Short term |
| 6 | Apache Version Exposed in Headers | LOW | Long term |

A total of **6 vulnerabilities** were identified during this assessment: 1 High, 3 Medium, and 2 Low severity findings.

## Methodology

The assessment followed a passive and non-destructive approach across three phases:

**1. Network Reconnaissance (Nmap)**
An Nmap scan was performed against 127.0.0.1 targeting common ports (21, 22, 80, 443, 8080). Port 80 was found open running Apache httpd 2.4.66 on Debian. All other scanned ports were closed.

**2. Web Vulnerability Scanning (OWASP ZAP)**
OWASP ZAP was configured as an HTTP proxy to intercept and analyze all traffic to and from http://localhost/dvwa/login.php. The passive scan detected 9 alerts grouped into 6 distinct findings covering injection flaws, missing security headers, and cookie misconfigurations.

**3. Manual Header Inspection (Browser DevTools)**
Firefox DevTools was used to manually inspect HTTP response headers on the login page. Several critical security headers were found to be absent, including Content-Security-Policy, X-Frame-Options, X-Content-Type-Options, and Strict-Transport-Security. The Server header was also found to expose the exact Apache version.

## Key Findings

### SQL Injection - HIGH
The application fails to sanitize user input before constructing SQL queries, making it vulnerable to SQL injection attacks. This represents the most critical finding as it could lead to complete database compromise.

### Missing Security Headers - MEDIUM
Multiple HTTP security headers are absent from server responses, including Content-Security-Policy and X-Frame-Options, leaving the application exposed to XSS and Clickjacking attacks.

### Cookie Misconfiguration - MEDIUM
The session cookie lacks the SameSite attribute, which increases exposure to cross-site request forgery attacks.

### Apache Version Disclosure - LOW
The Server response header reveals the exact version of Apache running on the server, providing useful reconnaissance information to potential attackers.

## Recommendations

1. Replace all dynamic SQL queries with parameterized queries (prepared statements) to eliminate SQL injection risk.
2. Implement Anti-CSRF tokens on all forms that perform state-changing operations.
3. Add a Content-Security-Policy header to all HTTP responses to mitigate XSS risk.
4. Configure X-Frame-Options: DENY on all responses to prevent Clickjacking.
5. Set SameSite=Strict on all session cookies alongside HttpOnly and Secure flags.
6. Configure ServerTokens Prod and ServerSignature Off in Apache to suppress version disclosure.

## Repository Structure
FUTURE_CS_01/
└── Task_1_Vulnerability_Assessment/
├── README.md
├── Vulnerability_Assessment_Report_DVWA.pdf
└── screenshots/
├── dvwa_homepage.png
├── nmap_results.png
├── owasp_zap_alerts.png
└── devtools_headers.png
## Disclaimer

This assessment was conducted exclusively on a deliberately vulnerable test application (DVWA) installed in a local environment. No production systems were targeted or affected. This work was completed as part of the Future Interns Cyber Security Internship program (Task 1).
