# OWASP Juice Shop – SQL Injection Leading to Authentication Bypass

## Target
OWASP Juice Shop (Local Instance)

## Vulnerability Type
SQL Injection (Authentication Bypass)

## Severity
High

## OWASP Category
- OWASP Top 10: A03 – Injection
- CWE-89: Improper Neutralization of Special Elements used in an SQL Command

---

## Description

The login functionality of the application is vulnerable to SQL Injection due to improper handling of user-supplied input in the authentication process. By injecting a crafted SQL payload into the email input field, an attacker can bypass authentication and gain unauthorized access to the administrator account.

The vulnerability exists because user input is directly concatenated into backend SQL queries without proper sanitization or parameterization.

---

## Attack Scenario

An attacker attempts to authenticate without valid credentials. By injecting a malicious SQL condition into the email field, the attacker manipulates the SQL query logic so that it always evaluates as true for an administrator account. As a result, the application grants administrative access without validating the password.

---

## Steps to Reproduce

1. Navigate to the login page of the application.
2. In the **Email** field, enter the following payload: **' OR email LIKE 'admin%' --**
3. Enter any random value in the **Password** field.
4. Submit the login form.
5. Observe that authentication succeeds and the attacker is logged in as the **administrator** account.

---

## Proof of Concept (PoC)

### Injected Payload:

- ' OR email LIKE 'admin%' --

### Backend Query Explanation

**A vulnerable SQL query may look like:**

SELECT * FROM Users
WHERE email = '<user_input>' AND password = '<password_input>';


**After injection, the query becomes:**

SELECT * FROM Users
WHERE email = '' OR email LIKE 'admin%' -- ' AND password = 'random';

The -- sequence comments out the password check, resulting in successful authentication.

## Impact

This vulnerability allows:

- Authentication bypass
- Unauthorized administrator access
- Full application compromise
- Potential data exposure, modification, or deletion

In a real-world production environment, exploitation of this issue would be considered **critical**, as it enables complete takeover of the application.

---

## Mitigation / Fix

To prevent this vulnerability, the following controls should be implemented:

- Use parameterized queries (prepared statements) for all database interactions.
- Avoid dynamic SQL query construction using user-supplied input.
- Implement ORM-based database access with built-in query protection.
- Enforce strict input validation and sanitization.
- Apply the principle of least privilege to database users.

---

## Learning Outcome

- SQL Injection remains one of the most dangerous and impactful web vulnerabilities.
- Authentication mechanisms are high-risk attack surfaces and must be carefully secured.
- Secure coding practices are essential to prevent complete system compromise.
most dangerous web vulnerabilities.

Authentication mechanisms are high-risk targets.

Secure coding practices are essential to prevent complete system compromise.
