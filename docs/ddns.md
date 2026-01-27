---
title: DDNS
status: done
---

DDNS is one of those networking concepts that sounds complex at first, but once you understand the problem it solves, everything clicks.

## 1. The Problem DDNS Solves

### IP addresses change

Most home and small-office Internet connections use a **dynamic public IP address**.

That means:

-   Today your public IP might be `203.0.113.25`
-   Tomorrow it might become `203.0.113.89`
-   You don’t control when it changes

This creates a problem:

>   **How can others always find your server if its IP keeps changing?**

## 2. What Is DDNS?

**DDNS (Dynamic DNS)** automatically keeps a **domain name** pointing to your **current public IP address**.

In simple terms:

>   **DDNS = a domain name that follows your changing IP**

## 3. DNS vs DDNS

| Feature       | DNS                 | DDNS                        |
| ------------- | ------------------- | --------------------------- |
| IP address    | Usually static      | Changes frequently          |
| Updates       | Manual              | Automatic                   |
| Typical users | Websites, companies | Home users, labs, servers   |
| Use case      | Public services     | Home servers, remote access |

## 4. A Real-World Example

Without DDNS:

```
You → remember IP address → IP changes → connection fails
```

With DDNS:

```
you.myhome.net → always updated → always reachable
```

Your device or router:

1.  Detects IP change
2.  Updates DDNS provider
3.  Domain points to new IP automatically

## 5. How DDNS Works (Step by Step)

1.  You register a domain or subdomain
    Example: `myserver.ddns.net`

2.  Your ISP assigns you a public IP
    Example: `100.64.12.45`

3.  A **DDNS client** runs on:

    -   Your router, or
    -   A computer/server

4.  When IP changes:

    -   Client sends update to DDNS provider
    -   DNS record is updated

5.  Users connect using:

    ```
    myserver.ddns.net
    ```

## 6. Common DDNS Providers

Popular beginner-friendly DDNS services:

-   No-IP
-   DuckDNS
-   Dynu
-   Cloudflare (via API)
-   Router-built-in DDNS services

Many home routers support DDNS directly.

## 7. DDNS Client: Where Does It Run?

### Option 1: On the Router (Recommended)

-   Works even when computers are off
-   Most stable solution

### Option 2: On a Computer or Server

-   Useful if router doesn’t support DDNS
-   Requires system to be always on

## 8. Typical DDNS Use Cases

### 1️⃣ Remote Access to Home Server

-   NAS
-   **Media** server
-   **Web** server
-   **Game** server

### 2️⃣ SSH into Home Network

```
ssh user@myhome.ddns.net
```

### 3️⃣ VPN Server

-   WireGuard
-   OpenVPN
-   Tailscale fallback endpoints

### 4️⃣ Learning & Labs

-   Networking practice
-   Self-hosted services

## 9. DDNS + Port Forwarding

DDNS alone is **not enough**.

You also need:

-   **Port forwarding** on your router

Example:

| Service | Port |
| ------- | ---- |
| SSH     | 22   |
| HTTP    | 80   |
| HTTPS   | 443  |

Flow:

```
Internet → Public IP → Router → Local Device
```

⚠️ Exposing ports requires **security awareness**.

## 10. DDNS vs Static IP

| Feature   | DDNS         | Static IP               |
| --------- | ------------ | ----------------------- |
| Cost      | Free / cheap | Often expensive         |
| Setup     | Easy         | ISP dependent           |
| Stability | Good         | Excellent               |
| Home use  | ✅            | ❌ (usually unnecessary) |

DDNS is the **practical choice** for most home users.

## 11. DDNS vs Tailscale / Zero-Trust Tools

| Feature         | DDNS         | Tailscale    |
| --------------- | ------------ | ------------ |
| Public exposure | Yes          | No           |
| Port forwarding | Required     | Not required |
| Security        | User-managed | Built-in     |
| Ease of use     | Medium       | Very easy    |

📌 DDNS is classic networking

📌 Tailscale is modern zero-trust networking

Both are useful to learn.

## 12. Basic Security Tips (Very Important)

✔ Use strong passwords

✔ Prefer HTTPS over HTTP

✔ Avoid exposing unnecessary ports

✔ Change default router credentials

✔ Consider a firewall or VPN

## 13. Minimal Example Setup (Conceptual)

```
[ Laptop ] → myserver.ddns.net → [ Router ]
                                     ↓
                               [ Home Server ]
```

Your domain always resolves to your current public IP.

## 14. Common Beginner Mistakes

❌ Thinking DDNS gives you Internet access

❌ Forgetting port forwarding

❌ Exposing services without security

❌ Running DDNS client on an unstable device

## 15. Summary

-   DDNS solves the **changing public IP problem**
-   It keeps a **domain name updated automatically**
-   Works best with router-based clients
-   Often used with port forwarding
-   Essential skill for home labs and self-hosting

