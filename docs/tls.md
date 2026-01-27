---
title: TLS
status: done
---

# TLS

## 1. Why TLS Exists

Imagine sending a postcard through the mail ✉️
Anyone along the way can:

-   Read it
-   Copy it
-   Modify it

Early Internet traffic worked like that.

**TLS fixes this problem** by providing:

1.  🔒 **Encryption** – others can’t read the data
2.  🧾 **Authentication** – you know who you’re talking to
3.  🛡 **Integrity** – data can’t be secretly modified

Today, TLS protects:

-   HTTPS (websites)
-   Email (SMTP, IMAP)
-   APIs
-   VPNs
-   Many CLI tools and cloud services

If you see `https://`, TLS is involved.

## 2. What Is TLS?

**TLS (Transport Layer Security)** is a cryptographic protocol that secures communication between two parties, usually:

-   **Client** (browser, app, CLI tool)
-   **Server** (website, API, service)

TLS operates **between the application layer and the transport layer**.

```
Application (HTTP, SMTP, IMAP)
------------------------------
TLS
------------------------------
TCP
------------------------------
IP
```

**TLS is the modern replacement for SSL (SSL is obsolete).**

## 3. What Problems TLS Solves

### 3.1 Eavesdropping

Without TLS:

```
password=123456
```

With TLS:

```
9f a3 7b 2c 81 ...
```

### 3.2 Man-in-the-Middle Attacks

TLS ensures:

-   You are really talking to **google.com**
-   Not an attacker pretending to be it

### 3.3 Data Tampering

TLS detects if data was modified in transit.

## 4. TLS in One Picture

```
Client                    Server
  | ------ ClientHello ---> |
  | <----- ServerHello ---- |
  | <--- Certificate ------ |
  | ---- Key Exchange ----> |
  | === Encrypted Data ===  |
```

This initial process is called the **TLS Handshake**.

## *5. The TLS Handshake (Step by Step)

Let’s simplify what really happens.

### Step 1: ClientHello

The client says:

-   TLS versions it supports
-   Encryption algorithms it supports
-   A random number

### Step 2: ServerHello

The server replies:

-   Chosen TLS version
-   Chosen encryption algorithm
-   Its own random number
-   **Certificate**

### Step 3: Certificate Verification

The client:

-   Checks the server certificate
-   Verifies it was signed by a trusted **Certificate Authority (CA)**

If this fails → connection aborted ❌

### Step 4: Key Exchange

Both sides:

-   Agree on a **shared secret**
-   No one else can compute it

(Modern TLS uses **ECDHE** for this.)

### Step 5: Secure Communication

All further data is:

-   Encrypted
-   Authenticated
-   Integrity-protected

## 6. Certificates Explained Simply

A **TLS certificate** binds:

```
Public Key ↔ Domain Name
```

Issued by a **Certificate Authority (CA)** like:

-   **Let’s Encrypt**
-   DigiCert
-   GlobalSign

Your OS/browser trusts a list of CAs.

### Certificate Chain

```
Your Site
  ↓
Intermediate CA
  ↓
Root CA (trusted by OS)
```

## 7. Symmetric vs Asymmetric Encryption

TLS uses **both**.

### Asymmetric Encryption (Slow)

-   Public key / private key
-   Used for:
    -   Authentication
    -   Key exchange

### Symmetric Encryption (Fast)

-   One shared secret key
-   Used for:
    -   Actual data transfer

TLS combines them:

>   Asymmetric to agree on a key, symmetric to send data.

## 8. Common TLS Versions

| Version | Status            |
| ------- | ----------------- |
| SSL 2.0 | ❌ Broken          |
| SSL 3.0 | ❌ Broken          |
| TLS 1.0 | ❌ Deprecated      |
| TLS 1.1 | ❌ Deprecated      |
| TLS 1.2 | ✅ Widely used     |
| TLS 1.3 | ✅ Modern & faster |

**TLS 1.3** improves:

-   Faster handshake
-   Better security defaults
-   Fewer insecure options

## 9. HTTPS = HTTP + TLS

```
HTTP   → Plain text
HTTPS  → HTTP over TLS
```

What HTTPS gives you:

-   🔒 Encrypted traffic
-   🧾 Verified website identity
-   🛡 Protection against tampering

What it does **not** give you:

-   A “safe” website
-   Protection from malware
-   Anonymity

## *10. TLS in Practice (CLI Examples)

### Check a TLS connection

```bash
openssl s_client -connect google.com:443
```

### See certificate details

```bash
openssl x509 -in cert.pem -text -noout
```

### Test supported TLS versions

```bash
curl -v https://example.com
```

## 11. Common Beginner Confusions

### “TLS encrypts everything on the server”

❌ No

TLS only protects **data in transit**, not at rest.

### “TLS makes a website trustworthy”

❌ No

It only proves domain ownership, not intent.

### “SSL and TLS are the same”

❌ SSL is obsolete; TLS is the correct term.

## 12. Where You’ll See TLS

-   Browsers (HTTPS)
-   Git over HTTPS
-   Docker registries
-   Email servers
-   APIs
-   Cloud services
-   **Tailscale**, VPNs, proxies

If security matters, TLS is there.

## 13. Mental Model to Remember

Think of TLS as:

>   **A locked, tamper-proof, identity-verified tunnel**
>   between your computer and a server.

## 14. What to Learn Next

If you want to go deeper:

-   How certificates are issued (ACME / **Let’s Encrypt**)
-   TLS 1.3 handshake in detail
-   Cipher suites
-   Mutual TLS (mTLS)
-   TLS in Nginx / Caddy