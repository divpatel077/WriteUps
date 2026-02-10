# OWASP Juice Shop – Horizontal IDOR in View Cart Functionality

## Target
OWASP Juice Shop (Local Instance)

## Vulnerability Type
Insecure Direct Object Reference (IDOR) – Horizontal Privilege Escalation

## Severity
High

## OWASP Category
- OWASP Top 10: A01 – Broken Access Control
- CWE-639: Authorization Bypass Through User-Controlled Key

---

## Description

The application’s **View Cart** functionality is vulnerable to a Horizontal Insecure Direct Object Reference (IDOR). The application uses a predictable cart identifier to fetch cart details but fails to properly verify whether the authenticated user is authorized to access the requested cart.

By modifying the cart ID parameter in the request, it is possible to view other customers’ shopping carts without authorization.

---

## Attack Scenario

An authenticated user accesses their own shopping cart. By intercepting the request and changing the cart identifier to another valid value, the attacker can retrieve cart data belonging to other users. This allows unauthorized access to sensitive user information and purchase details.

---

## Steps to Reproduce

1. Log in as a normal user.
2. Navigate to the **View Cart** functionality.
3. Intercept the request using browser Developer Tools or a proxy.
4. Identify the cart identifier parameter (e.g., `cartId`).
5. Modify the `cartId` value to another valid identifier.
6. Forward the request to the server.
7. Observe that the cart details of another user are displayed.

---

## Proof of Concept (PoC)

### Original Request

GET /rest/basket/1 HTTP/1.1
Host: juice-shop
Authorization: Bearer <user_token>

### Modified Request

GET /rest/basket/2 HTTP/1.1
Host: juice-shop
Authorization: Bearer <user_token>

### Result

The server responds with cart details belonging to a different user, confirming the presence of a Horizontal IDOR vulnerability.

---

## Impact

### This vulnerability allows:
- Unauthorized access to other users’ shopping cart data
- Exposure of sensitive customer information
- Violation of user privacy
- Potential manipulation of cart contents if combined with write actions

---

## Mitigation / Fix

### To remediate this vulnerability, the following measures should be implemented:

- Enforce server-side authorization checks for every object access.
- Validate ownership of the cart against the authenticated user.
- Avoid exposing direct object identifiers in requests.
- Use indirect references or UUIDs where possible.
- Apply defense-in-depth by validating authorization at both API and business-logic layers.

---

## Learning Outcome

- IDOR vulnerabilities are a common result of missing authorization checks.
- Authentication alone does not guarantee proper access control.
- Every request involving object identifiers must be validated on the server side.
- Horizontal privilege escalation can be just as damaging as vertical escalation.