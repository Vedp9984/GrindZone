## 🧨 4. Common Vulnerabilities

### 4.1 SQL Injection

* **Attack**: Injecting malicious SQL code via input fields to manipulate database queries.
* **Example**:

  ```sql
  SELECT * FROM users WHERE username = '' OR '1'='1';
  ```
* **Prevention**:

  * Use **Prepared Statements** or ORM libraries
  * Sanitize and validate all user input

### 4.2 Cross-Site Scripting (XSS)

* **Attack**: Injecting malicious JavaScript into a trusted web page viewed by others.
* **Types**:

  * Stored XSS
  * Reflected XSS
  * DOM-based XSS
* **Example**:

  ```html
  <script>alert('Hacked');</script>
  ```
* **Prevention**:

  * Escape HTML output properly
  * Use **Content Security Policy (CSP)**
  * Sanitize and validate user input

### 4.3 CSRF (Cross-Site Request Forgery)

* **Attack**: Tricks a logged-in user into submitting unintended requests to a web application.
* **Prevention**:

  * Use anti-CSRF tokens in forms
  * Use `SameSite` attribute in cookies
  * Implement **double-submit cookie pattern**

### 4.4 Broken Authentication

* **Issue**: Predictable session IDs, poor password storage, or insecure login mechanisms.
* **Prevention**:

  * Use strong password hashing (e.g., **bcrypt**, **scrypt**)
  * Implement session timeouts and account lockouts
  * Use **Multi-Factor Authentication (MFA)**

### 4.5 Insecure Deserialization

* **Attack**: Deserializing untrusted data may lead to **Remote Code Execution (RCE)**.
* **Prevention**:

  * Avoid deserializing data from untrusted sources
  * Prefer safer formats like **JSON** over binary formats
  * Use libraries that enforce safe serialization

---
