## 🛂 2. OAuth & JWT

### OAuth 2.0

OAuth 2.0 is a framework for **delegated authorization**, allowing a third-party application to access user resources **without exposing credentials**.

#### 🔑 Key Roles:

* **Resource Owner**: The user who owns the data
* **Client**: The application requesting access to the user's data
* **Authorization Server**: Issues access and refresh tokens after authenticating the resource owner
* **Resource Server**: Hosts the protected resources (e.g., APIs)

#### 🔁 Common OAuth Flows:

* **Authorization Code Flow** (Most secure, used in web apps)
* **Client Credentials Flow** (Used in machine-to-machine communication)
* **Implicit Flow** (Used in legacy SPAs; now discouraged)
* **Password Grant Flow** (Discouraged due to security risks)

#### 🔐 Tokens:

* **Access Token**: Short-lived token used to access resources
* **Refresh Token**: Long-lived token used to obtain a new access token without re-authenticating

---

### JWT (JSON Web Token)

JWT is a compact, URL-safe token format used for **stateless authentication and authorization**.

#### 🔍 Structure:

A JWT consists of three base64-encoded parts separated by dots:

```
Header.Payload.Signature
```

* **Header**: Contains metadata like token type ("JWT") and algorithm (e.g., HS256)
* **Payload**: Contains "claims" — data like user ID, roles, expiration, issuer
* **Signature**: Ensures the token was not tampered with. Created using the header, payload, and a secret key.

#### 📦 JWT Usage:

* Sent in HTTP headers: `Authorization: Bearer <token>`
* Commonly stored in:

  * `localStorage` (prone to XSS)
  * `sessionStorage`
  * **HTTP-only cookies** (recommended for security)

#### ⚠️ JWT Vulnerabilities:

* **"alg: none" Attack**: Accepting unsigned tokens due to misconfigured libraries
* **Token Leakage**: JWTs in localStorage are vulnerable to XSS
* **Long Expiry**: Use short-lived tokens + refresh mechanism

---

## 🧍 3. Authentication vs Authorization

| Concept       | Authentication             | Authorization          |
| ------------- | -------------------------- | ---------------------- |
| Meaning       | Who are you?               | What can you do?       |
| Purpose       | Verify identity            | Grant permissions      |
| Performed by  | Login systems, biometrics  | Access control systems |
| Occurs First? | ✅ Yes                      | ✅ After authentication |
| Data Used     | Username, password, tokens | Roles, policies, ACLs  |

### 🔐 Authentication Methods:

* Username + password
* OTP, TOTP (e.g., Google Authenticator)
* OAuth 2.0 / OpenID Connect
* Biometric: Fingerprint, facial recognition

### 🛡️ Authorization Methods:

* **RBAC** (Role-Based Access Control): Permissions assigned to roles
* **ABAC** (Attribute-Based Access Control): Based on attributes like location, time
* **ACL** (Access Control List): Permissions tied to specific resources

---

