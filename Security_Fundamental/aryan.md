# 1. Encryption (SSL/TLS)

---

## 1.1 What Is Encryption?
- **Encryption**: Transforming plaintext into ciphertext so that only authorized parties can read it.
- **Goals:**
  - **Confidentiality**: Only intended recipients can decipher the data.
  - **Integrity**: Detect any alteration of the data.
  - **Authentication**: Verify the identity of the parties involved.

---

## 1.2 Symmetric vs. Asymmetric Encryption

| Aspect               | Symmetric                                 | Asymmetric                                    |
|----------------------|-------------------------------------------|-----------------------------------------------|
| Key used             | Single shared secret                      | Key pair: public key & private key            |
| Speed                | Fast                                      | Slower (computationally expensive)            |
| Use cases            | Bulk data encryption (e.g., AES-GCM)      | Key exchange, digital signatures (e.g., RSA)  |
| Key distribution     | Harder (secure channel required)          | Easier (public keys can be freely distributed)|

---

## 1.3 SSL vs. TLS

- **SSL (Secure Sockets Layer)**: Older protocol (now deprecated; SSL 3.0 retired due to POODLE).
- **TLS (Transport Layer Security)**: Successor to SSL, current standard for secure web traffic.
  - Major versions: TLS 1.0 → 1.1 → 1.2 → **1.3** (recommended).
  - Improvements in TLS 1.3:
    - Reduced handshake round‐trips (faster).
    - Removed outdated/weak ciphers.
    - Enforced forward secrecy by default.

---

## 1.4 TLS Handshake Overview

### 1.4.1 TLS 1.2 Handshake
1. **ClientHello**  
   - Sends supported TLS version, cipher suites, random nonce.
2. **ServerHello**  
   - Picks version & cipher suite, sends its random nonce + certificate.
3. **Certificate**  
   - X.509 certificate chain (server’s public key + CA signature).
4. **ServerKeyExchange** *(if needed)*  
   - Parameters for Diffie–Hellman, ECDHE, etc.
5. **ServerHelloDone**  
   - Signals end of server messages.
6. **ClientKeyExchange**  
   - Sends PreMasterSecret encrypted with server’s public key (RSA) or DH parameters.
7. **ChangeCipherSpec** (Client → Server)  
   - Signals switch to encrypted mode.
8. **Finished** (Client → Server)  
   - Verifies handshake integrity.
9. **ChangeCipherSpec** (Server → Client)  
10. **Finished** (Server → Client)

### 1.4.2 TLS 1.3 Handshake (Simplified)
1. **ClientHello**  
   - Includes supported cipher suites, key-share (ECDHE), extensions.
2. **ServerHello**  
   - Selects cipher suite, key-share; sends its certificate & proof.
3. **Encrypted Extensions**  
   - Further handshake data (e.g., ALPN).
4. **Certificate Request** *(optional)*  
5. **Certificate**  
6. **Certificate Verify**  
7. **Finished**  
- **0-RTT**: Client may send early data if resuming a previous session.

---

## 1.5 Certificates & Public Key Infrastructure (PKI)
- **X.509 Certificate**  
  - Contains: Subject, Issuer, Public Key, Validity period, Extensions, Signature.
- **Certificate Authority (CA)**  
  - Trusted third party that signs & issues certificates.
- **Chain of Trust**  
  - End-entity cert → Intermediate CA(s) → Root CA.
- **Revocation**  
  - CRL (Certificate Revocation List) or OCSP (Online Certificate Status Protocol).

---

## 1.6 Cipher Suites
- Formatted as: `TLS_<version>_<KeyExchange>_<Cipher>_<MAC_or_Mode>`
  - e.g. `TLS_AES_128_GCM_SHA256` (TLS 1.3)
  - e.g. `ECDHE-RSA-AES256-GCM-SHA384` (TLS 1.2)
- **KeyExchange**: RSA, DHE, ECDHE (for forward secrecy)
- **Cipher**: AES, ChaCha20, …  
- **Mode/MAC**: GCM (AEAD), CBC+HMAC, SHA256, SHA384

---

## 1.7 Perfect Forward Secrecy (PFS)
- Achieved via **(EC)DHE** key exchange.
- Ensures past sessions cannot be decrypted even if long‐term keys are compromised.

---

## 1.8 TLS Record Protocol
1. **Fragmentation**: Break data into manageable blocks.
2. **Compression**: (Rarely used; often disabled)
3. **MAC**: Compute message authentication code.
4. **Encryption**: Encrypt the MAC’d plaintext.
5. **Transmission**: Prepend sequence numbers & content type.

---

## 1.9 Common Attacks & Mitigations
| Attack                    | Description                                      | Mitigation                      |
|---------------------------|--------------------------------------------------|---------------------------------|
| **POODLE**                | SSL 3.0 downgrade + padding oracle               | Disable SSL 2/3, use TLS 1.2+   |
| **BEAST**                 | TLS 1.0 CBC vulnerability                        | Use TLS 1.2+, GCM modes         |
| **Heartbleed**            | OpenSSL heartbeat buffer over‐read               | Patch OpenSSL, disable heartbeat|
| **Weak Ciphers**          | RC4, 3DES vulnerabilities                        | Disable RC4/3DES, use AEAD      |
| **Certificate Forgery**   | Rogue CA issue or MITM                           | Pin certificates, OCSP Stapling |

---

> **Next:**  
> Dive into **OAuth & JWT**, their roles in modern web authorization flows. 🛡️  

# 2. OAuth & JWT

---

## 2.1 OAuth 2.0

### What Is OAuth?
- **OAuth 2.0** is an authorization framework that lets third-party apps obtain limited access to a user’s resources without sharing the user’s credentials.
- **Roles**  
  - **Resource Owner** (User)  
  - **Client** (App requesting access)  
  - **Authorization Server** (Issues tokens)  
  - **Resource Server** (Hosts protected resources)

### Common Grant Types

| Grant Type                   | When to Use                                           |
|------------------------------|-------------------------------------------------------|
| **Authorization Code**       | Web apps with a backend server (most secure)          |
| **Implicit**                 | Single-page apps (deprecated in OAuth 2.1)            |
| **Client Credentials**       | Server-to-server (no user involved)                   |
| **Resource Owner Password**  | Trusted first-party apps (user shares credentials)    |

### Example: Authorization Code Flow

1. **Authorization Request**  
   ```http
   GET /authorize?
     response_type=code
     &client_id=abc123
     &redirect_uri=https://app.example.com/callback
     &scope=read_profile
     &state=xyz
   Host: auth.example.com

2. **Redirect with Authorization Code**  
   ```http
    HTTP/1.1 302 Found
    Location: https://app.example.com/callback?
          code=SplxlOBeZQQYbYS6WxSbIA
          &state=xyz
    ``` 
3. **Token Request**  
   ```http
    POST /token HTTP/1.1
    Host: auth.example.com
    Authorization: Basic BASE64(client_id:client_secret)
    Content-Type: application/x-www-form-urlencoded

    grant_type=authorization_code
    &code=SplxlOBeZQQYbYS6WxSbIA
    &redirect_uri=https://app.example.com/callback
    ```

4. **Token Response**  
   ```json
    {
    "access_token": "SlAV32hkKG",
    "token_type": "Bearer",
    "expires_in": 3600,
    "refresh_token": "tGzv3JOkF0XG5Qx2TlKWIA",
    "scope": "read_profile"
    }
    ```