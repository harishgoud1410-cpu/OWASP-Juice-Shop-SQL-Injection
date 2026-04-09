# OWASP-Juice-Shop-SQL-Injection
SQL Injection leading to Admin Account Takeover (OWASP Juice Shop)
OWASP-Juice-Shop-SQL-Injection/
│
├── README.md
├── SQLi-Admin-Takeover-Juice-Shop.pdf
├── screenshots/
│   ├── sqli_payload.png
│   ├── burp_response.png
│   └── admin_access.png
├── poc.txt# SQL Injection → Admin Account Takeover (OWASP Juice Shop)

## Overview

This project demonstrates a critical SQL Injection vulnerability in the OWASP Juice Shop application, leading to authentication bypass and full administrative account takeover.

---

## Vulnerability

**Type:** SQL Injection (Authentication Bypass)
**OWASP Category:** A03: Injection
**Severity:** Critical

---

## Attack Chain

1. Intercept login request using Burp Suite
2. Inject SQL payload to bypass authentication
3. Extract valid admin user information
4. Perform credential validation
5. Gain full administrative access

---

## Proof of Concept (PoC)

### Payload Used:

```
admin' OR '1'='1'--
```

### Request Endpoint:

```
POST /rest/user/login
```

### Result:

* Server returned HTTP 200
* Authentication token issued
* Login successful without valid credentials

---

## Exploitation Evidence

* Authentication bypass confirmed
* JWT token received
* Admin-level access achieved

(Refer to `screenshots/` for detailed proof)

---

## Impact

* Full admin account takeover
* Unauthorized access to sensitive data
* Potential data manipulation and privilege abuse

---

## Tools Used

* Burp Suite (Proxy, Repeater)
* Manual Testing

---

## Repository Contents

* `SQLi-Admin-Takeover-Juice-Shop.pdf` → Full detailed report
* `screenshots/` → Exploitation proof
* `poc.txt` → Step-by-step reproduction

---

## Skills Demonstrated

* SQL Injection Exploitation
* Authentication Bypass
* User Enumeration
* Attack Chaining
* Professional Security Reporting

---

## Disclaimer

This project was performed in a controlled lab environment (OWASP Juice Shop) for educational purposes only.
