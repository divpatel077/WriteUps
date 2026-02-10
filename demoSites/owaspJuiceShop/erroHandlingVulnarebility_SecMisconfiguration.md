# OWASP Juice Shop – Error Handling Vulnerability

## Target
OWASP Juice Shop (Local Instance)

## Vulnerability Type
Improper Error Handling / Information Disclosure

## Severity
Low

## OWASP Category
- OWASP Top 10: A05 – Security Misconfiguration  
- Related: Improper Exception Handling

---

## Description

The application exposes detailed internal error messages and stack traces when unexpected or malformed API paths are accessed. By manipulating an existing API request and sending it to an unintended endpoint, the server returns verbose error information including internal file paths, route handling logic, and framework details.

This indicates improper error handling and insufficient production hardening.

---

## Attack Scenario

An attacker modifies a legitimate API request and sends it to an unauthorized or unexpected REST endpoint. Instead of returning a generic error response, the server discloses internal implementation details through stack traces. These details can help attackers understand backend routing, framework usage, and internal file structure, which can be leveraged for further attacks.

---

## Steps to Reproduce

1. Intercept any valid API request using browser Developer Tools or a proxy tool.
2. Modify the request path to an unexpected administrative endpoint: **"/rest/admin"**
3. Send the modified request to the server.
4. Observe the verbose error response returned by the application.

---

The server responds with a detailed stack trace similar to the following:

## Proof of Concept (PoC)Unexpected path: /rest/admin
at /juice-shop/build/routes/angular.js:42:18
at Layer.handle [as handle_request] (/juice-shop/node_modules/express/lib/router/layer.js:95:5)
at trim_prefix (/juice-shop/node_modules/express/lib/router/index.js:328:13)
at /juice-shop/node_modules/express/lib/router/index.js:286:9
at Function.process_params (/juice-shop/node_modules/express/lib/router/index.js:346:12)
at next (/juice-shop/node_modules/express/lib/router/index.js:280:10)


The response reveals:
- Internal directory and file structure
- Express.js routing and middleware flow
- Backend framework implementation details

This behavior completes the **Error Handling** challenge in OWASP Juice Shop.

---

## Impact

Although this vulnerability does not directly lead to authentication bypass or data exposure, it provides valuable reconnaissance information such as:
- Backend technology stack
- Internal routing logic
- File system paths

Such information can significantly aid attackers in chaining more severe vulnerabilities.

---

## Mitigation / Fix

- Disable verbose error messages in production environments.
- Return generic HTTP error responses (e.g., 404 or 500) without stack traces.
- Log detailed errors only on the server side.
- Implement centralized error-handling middleware with environment-based controls.

---

## Learning Outcome

- Error handling vulnerabilities often act as enablers for larger attacks.
- Unexpected input paths should never expose internal implementation details.
- Proper production hardening is essential even for low-severity issues.

