# OWASP Juice Shop – Reflected XSS via Search Functionality

## Target
OWASP Juice Shop (Local Instance)

## Vulnerability Type
Reflected Cross-Site Scripting (XSS)

## Severity
Medium

## OWASP Category
- OWASP Top 10: A03 – Injection
- CWE-79: Improper Neutralization of Input During Web Page Generation

---

## Description

The search functionality of the application is vulnerable to Reflected Cross-Site Scripting (XSS). User-supplied input from the search bar is reflected back into the HTML response without proper sanitization or encoding. As a result, an attacker can inject and execute arbitrary JavaScript code in the victim’s browser.

---

## Attack Scenario

An attacker crafts a malicious payload and injects it into the search field. When the application reflects this input back to the page, the browser interprets and executes the injected JavaScript. If a victim is tricked into clicking a malicious link containing this payload, the attacker can execute code in the victim’s session context.

---

## Steps to Reproduce

1. Navigate to the application’s search bar.
2. Enter the following payload into the search input:
   ```html
   <iframe src="javascript:alert('xss')"></iframe>
3. Submit the search request.
4. Observe that a JavaScript alert box is executed in the browser.

---

## Proof of Concept (PoC)

### Payload Used: 
- ```html
    <iframe src="javascript:alert('xss')"></iframe>

### Explanation

- The search functionality reflects user-supplied input directly into the HTML response without proper sanitization or encoding.
- When the injected `<iframe>` element is rendered by the browser, it is parsed as valid HTML.
- The `src` attribute uses the `javascript:` URI scheme instead of a normal URL.
- Browsers interpret `javascript:` URIs as executable JavaScript code.
- As a result, the JavaScript payload `alert('xss')` is executed in the context of the application.
- This confirms the presence of a Reflected Cross-Site Scripting (XSS) vulnerability.

---

## Impact

This vulnerability allows:

- Execution of arbitrary JavaScript in the victim’s browser
- Session hijacking if session cookies are not marked as `HttpOnly`
- Theft of sensitive information such as authentication tokens
- Manipulation of page content for phishing or social engineering attacks

In a real-world application, exploitation of this issue could lead to user account compromise and loss of trust.

---

## Mitigation / Fix

To prevent this vulnerability, the following controls should be implemented:

- Apply context-aware output encoding for all user-supplied data.
- Sanitize and validate input received from the search functionality.
- Disallow dangerous URI schemes such as `javascript:`, `data:`, and `vbscript:`.
- Implement a strong Content Security Policy (CSP).
- Use modern frontend frameworks that provide built-in XSS protection.

---

## Learning Outcome

- Search fields are common and high-risk injection points for XSS attacks.
- Reflected XSS can be exploited through simple user interaction such as clicking a malicious link.
- Proper output encoding is more effective than blacklisting individual characters or tags.
- Defense-in-depth controls such as CSP significantly reduce XSS impact.
