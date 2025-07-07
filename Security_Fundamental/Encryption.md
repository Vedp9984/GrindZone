## 🔐 1. Encryption (SSL/TLS)

### What is Encryption?

Encryption is the process of converting **plaintext** (readable data) into **ciphertext** (unreadable data) using algorithms and keys. This ensures that only **authorized parties** with the correct decryption key can access the original data.

---

### Types of Encryption

#### ✉️ Symmetric Encryption

* Uses **the same key** for both encryption and decryption.
* Faster and efficient for large data.
* Example: **AES (Advanced Encryption Standard)**
* Key management is challenging, as both sender and receiver need the same key securely.

#### 🔑 Asymmetric Encryption

* Uses a **public key** to encrypt and a **private key** to decrypt.
* Example: **RSA (Rivest-Shamir-Adleman)**
* Slower but more secure key exchange.
* Public key can be shared openly; private key must be kept secret.

---

### SSL/TLS Protocol

**SSL (Secure Sockets Layer)** and **TLS (Transport Layer Security)** are cryptographic protocols that provide **secure communication over the internet**, such as in HTTPS.

* **SSL is deprecated**, replaced by **TLS**, which is more secure and modern.
* Protects data integrity, confidentiality, and authenticity.
* Commonly used in web browsers, email, messaging apps, etc.

---

### TLS Handshake: Step-by-Step

The TLS handshake is the process where client and server agree on how to communicate securely:

1. **Client Hello**:

   * Client sends a hello message with supported TLS versions and cipher suites.

2. **Server Hello**:

   * Server responds with chosen cipher suite and sends its **digital certificate** (includes public key).

3. **Certificate Verification**:

   * Client verifies server's certificate using a **Certificate Authority (CA)**.

4. **Session Key Exchange**:

   * Client generates a **session key**, encrypts it with server's **public key**, and sends it.

5. **Key Derivation**:

   * Both client and server derive a **shared session key** for symmetric encryption.

6. **Secure Communication Begins**:

   * All further communication is encrypted using the derived session key.

---

### Key Terms to Know

| Term                           | Description                                                          |
| ------------------------------ | -------------------------------------------------------------------- |
| **HTTPS**                      | Secure version of HTTP using TLS                                     |
| **Certificate Authority (CA)** | Trusted entity that issues digital certificates                      |
| **Digital Certificate**        | Used to verify ownership of public key                               |
| **Cipher Suite**               | Defines encryption, authentication, and key exchange algorithms used |
| **Forward Secrecy**            | Ensures a new session key is used for every session                  |
| **Public/Private Key Pair**    | Used in asymmetric encryption for secure key exchange                |

---

### Common Tools

* **OpenSSL Command**: Check SSL/TLS connection details

  ```bash
  openssl s_client -connect example.com:443
  ```

* **Browser Lock Icon**:

  * Click it to view certificate, issuer, expiry, and encryption details.

* **Wireshark**:

  * Network analyzer to inspect encrypted TLS traffic (pre-handshake visible).

---

### Summary

* **Encryption** is essential for securing data.
* **TLS** is the modern standard for secure communication.
* TLS handshake ensures a secure, encrypted channel before data is exchanged.
* Always use **HTTPS** for web communication to protect user data.

---

