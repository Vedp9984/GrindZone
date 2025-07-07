# 8. Security Fundamentals

This document covers core topics in web and application security fundamentals, especially relevant for interviews and real-world application development.

---

## 🔐 1. Encryption (SSL/TLS)

### What is Encryption?

Encryption is the process of converting plaintext into ciphertext using algorithms and keys, so that only authorized parties can access the original data.

### Types of Encryption

* **Symmetric Encryption** (e.g., AES): Same key for encryption and decryption.
* **Asymmetric Encryption** (e.g., RSA): Uses public/private key pair.

### SSL/TLS Protocol

* SSL (deprecated) and TLS are cryptographic protocols that secure communications over the Internet.

### TLS Handshake Steps:

1. Client sends a "Client Hello" with supported cipher suites.
2. Server responds with its certificate and public key.
3. Client verifies certificate, sends a session key encrypted with the server's public key.
4. Both sides derive the same session key.
5. Secure communication begins.

### Key Terms:

* **HTTPS** = HTTP + TLS
* **Certificate Authorities (CA)**: Issue digital certificates
* **Forward Secrecy**: Session key changes with every new session

### Tools:

* `openssl s_client -connect example.com:443`
* Browser lock icon (certificate details)

---

## 🛂 2. OAuth & JWT

### OAuth 2.0

A framework for delegated authorization.

#### Key Roles:

* **Resource Owner**: The user
* **Client**: App requesting access
* **Authorization Server**: Issues tokens
* **Resource Server**: API or backend

#### Common Flows:

* **Authorization Code Flow** (most secure)
* **Client Credentials Flow** (for server-to-server)

#### Tokens:

* **Access Token**: Short-lived, used to access APIs
* **Refresh Token**: Used to obtain new access tokens

### JWT (JSON Web Token)

Used for stateless authentication.

#### Structure:

```
Header.Payload.Signature
```

* **Header**: Algorithm and token type
* **Payload**: Claims (user ID, roles, exp, etc.)
* **Signature**: Verifies integrity

#### JWT Usage:

* Sent in Authorization header: `Bearer <token>`
* Stored in localStorage, sessionStorage, or HTTP-only cookies

#### Vulnerabilities:

* `alg: none` attack (never trust input)
* Leaked tokens (no expiration)

---

## 🧍 3. Authentication vs Authorization

| Concept       | Authentication             | Authorization             |
| ------------- | -------------------------- | ------------------------- |
| Meaning       | Who are you?               | What can you do?          |
| Performed by  | Login systems, biometrics  | Role-based access control |
| Occurs First? | ✅ Yes                      | ✅ After authentication    |
| Data Used     | Username, password, tokens | Roles, policies, ACLs     |

### Authentication Methods:

* Username + password
* OTP, TOTP (Google Authenticator)
* OAuth/OpenID Connect
* Biometric (fingerprint, facial recognition)

### Authorization Methods:

* **RBAC**: Role-Based Access Control
* **ABAC**: Attribute-Based Access Control
* ACL: Access Control Lists

---

## 🧨 4. Common Vulnerabilities

### 4.1 SQL Injection

* Attack: Injecting SQL code via input fields
* Example:

```sql
SELECT * FROM users WHERE username = '' OR '1'='1';
```

* Prevention:

  * Use **Prepared Statements** / ORM
  * Sanitize and validate input

### 4.2 Cross-Site Scripting (XSS)

* Injecting malicious JavaScript into a trusted web page
* Types:

  * Stored
  * Reflected
  * DOM-based
* Example:

```html
<script>alert('Hacked');</script>
```

* Prevention:

  * Escape output
  * Use Content Security Policy (CSP)
  * Sanitize user input

### 4.3 CSRF (Cross-Site Request Forgery)

* Tricks user into submitting unwanted actions
* Prevention:

  * CSRF tokens in forms
  * SameSite cookies
  * Double submit cookie pattern

### 4.4 Broken Authentication

* Predictable sessions, improper password storage
* Prevention:

  * Use bcrypt/scrypt for password hashing
  * Implement session timeouts
  * MFA (Multi-Factor Authentication)

### 4.5 Insecure Deserialization

* Leads to Remote Code Execution (RCE)
* Prevention:

  * Avoid unserializing untrusted data
  * Use safe serializers (e.g., JSON over binary)

---

## 🛠️ Tools & Best Practices

### Tools:

* **Burp Suite**, **OWASP ZAP** (for penetration testing)
* **Postman** (for token validation)
* **jwt.io** (for decoding JWTs)

### Best Practices:

* Always use HTTPS (TLS)
* Store passwords securely (bcrypt)
* Use secure headers (CSP, X-Frame-Options)
* Sanitize and validate **all** input
* Implement logging and alerting for unusual activity
* Follow the **Principle of Least Privilege**

---

## 🎯 Sample Interview Questions

1. Explain the difference between authentication and authorization.
2. How does HTTPS work under the hood?
3. What is a JWT? How is it validated?
4. What is the OAuth 2.0 Authorization Code flow?
5. What are SQL injections and how do you prevent them?
6. What’s the difference between stored and reflected XSS?
7. How do you prevent CSRF in a web application?
8. Why is TLS preferred over SSL today?
9. Can JWT be revoked?
10. How would you secure a login API endpoint?

---

> Security is not a one-time implementation. It's a continuous process of assessment, mitigation, and monitoring.
