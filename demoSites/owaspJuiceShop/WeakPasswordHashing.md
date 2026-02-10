# OWASP Juice Shop – Weak Password Hashing and Sensitive Data Exposure via JWT

---

## Target
OWASP Juice Shop (Local Instance)



## Vulnerability Type
- Weak Cryptographic Hashing
- Sensitive Information Disclosure
- Insecure Token Design


## Severity
- **High**


## OWASP Category
- OWASP Top 10: A02 – Cryptographic Failures
- OWASP Top 10: A04 – Insecure Design
- CWE-327: Use of a Broken or Risky Cryptographic Algorithm
- CWE-522: Insufficiently Protected Credentials

---

## Description

The application uses a weak hashing algorithm for password storage and exposes the password hash inside a client-side JSON Web Token (JWT). The token is shared with the client via cookies and can be easily decoded without any secret using publicly available tools.

Because the hashing algorithm is weak and unsalted, the extracted password hash can be cracked using online hash databases, resulting in disclosure of the administrator’s plaintext password.

---

## Attack Scenario

An attacker first gains administrative access through a previously identified SQL Injection vulnerability. While authenticated as an administrator, the attacker observes that a JWT token is issued and stored in cookies for subsequent requests.

By decoding the JWT, the attacker extracts the administrator’s password hash. Since the application uses a weak hashing algorithm, the hash can be cracked using public hash-cracking databases, revealing the administrator’s actual password.

This allows permanent account compromise and credential reuse attacks.

---

## Steps to Reproduce

1. Log in as an administrator (e.g., via SQL Injection).
2. Inspect application cookies or request headers.
3. Identify the JWT token used for authentication.
4. Decode the JWT using a public decoder such as `jwt.io`.
5. Extract the password hash from the decoded token payload.
6. Submit the hash to an online hash-cracking service (e.g., hashes.com).
7. Observe that the administrator’s plaintext password is successfully recovered.

---

## Proof of Concept (PoC)

### JWT Token Extraction

**Authorization: Bearer <JWT_TOKEN>**

###Decoded JWT Payload (Sensitive Fields)
{
  "email": "admin@juice-sh.op",
  "password": "<password_hash>",
  "role": "admin"
}

### Hash Cracking Result

The extracted password hash was successfully cracked using an online hash database, revealing the administrator’s plaintext password.

---

## Impact

### This vulnerability allows:

- Disclosure of administrator password hashes
- Recovery of plaintext passwords
- Full and persistent administrative account compromise
- Credential reuse attacks across other platforms
- Complete breakdown of authentication security

In real-world applications, this issue represents a critical security failure.

---

## Mitigation / Fix

### To remediate this vulnerability, the following controls should be implemented:

- Use strong, adaptive password hashing algorithms such as bcrypt, scrypt, or Argon2.
- Apply unique salts to every password hash.
- Never store password hashes or sensitive credentials inside JWTs.
- Minimize JWT payloads to non-sensitive identifiers only.
- Secure tokens using HttpOnly, Secure, and SameSite cookie attributes.
- Perform regular cryptographic reviews and audits.

---

## Learning Outcome

- Weak hashing algorithms can completely undermine authentication security.
- JWTs are not encrypted by default and should never store sensitive data.
- Cryptographic failures often enable easy chaining with other vulnerabilities.
- Proper password storage is a foundational requirement for secure systems.