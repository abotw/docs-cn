# Ports

## 1. Start with a Simple Analogy 🚪

Imagine:

-   **IP address** = a building’s street address
-   **Port** = a specific door or room inside that building

Your computer may run **many services at the same time**:

-   a web server
-   an SSH server
-   a database
-   a game server

They all share **one IP address**, so how does the computer know **which service should receive incoming data**?

👉 **Ports solve this problem.**

## 2. What Is a Port?

A **port** is a **number** that identifies **a specific application or service** on a computer.

-   Range: **0 – 65535**
    -   $2^16$ = 65536
    -   16 bits
-   Each port can be “listened to” by **one service**
-   Data sent to an IP + port goes to **one exact program**

### Example

```
IP:   192.168.1.10
Port: 80
```

Means:

>   “Send this data to the web server running on this computer.”

## 3. Why Ports Are Necessary

Without ports:

-   Only **one service** could run per IP
-   Web + SSH + database would conflict

With ports:

-   Many services coexist safely

| Service            | Port |
| ------------------ | ---- |
| Web (HTTP)         | 80   |
| Secure Web (HTTPS) | 443  |
| SSH                | 22   |
| FTP                | 21   |
| MySQL              | 3306 |

## 4. IP Address vs Port (Very Important)

They always come **together**.

```
<IP address>:<Port>
```

Examples:

```
127.0.0.1:8080
192.168.1.5:22
google.com:443
```

Think of it as:

>   **City + house number**

## 5. Well-Known Ports (0–1023)

-   $2^10 =1024$, 10 bits

These are **standard ports**, reserved for common services.

| Port    | Service | Description            |
| ------- | ------- | ---------------------- |
| 20 / 21 | FTP     | File transfer          |
| 22      | SSH     | Remote login           |
| 25      | SMTP    | Email sending          |
| 53      | DNS     | Domain name resolution |
| 80      | HTTP    | Web (no encryption)    |
| 443     | HTTPS   | Web (encrypted)        |

⚠️ On Linux/macOS:

-   Ports **below 1024** usually require **root/admin** **privileges**

## 6. Registered & Dynamic Ports

### 1024–49151: Registered ports

-   Used by applications (databases, dev tools)

### 49152–65535: Dynamic (Ephemeral) ports

-   Automatically chosen by the OS
-   Used temporarily by clients

Example:

-   Browser connects to `google.com:443`
-   Your OS picks a random local port like `53124`

## 7. Server vs Client Ports

### Server

-   **Listens** on a fixed port
-   Example:

```
Web server listening on port 80
```

### Client

-   Uses a **random temporary port**
-   Example:

```
Client port: 53124 → Server port: 443
```

Diagram:

```
[Your Browser:53124] ---> [Server:443]
```

## 8. TCP Ports vs UDP Ports

Ports exist for **both TCP and UDP**, but they behave differently.

### TCP (Reliable)

-   Ordered
-   Retransmits lost data
-   Used by:
    -   HTTP/HTTPS
    -   SSH
    -   FTP

### UDP (Fast)

-   No guarantee of delivery
-   Lower latency
-   Used by:
    -   DNS
    -   Video streaming
    -   Online games

⚠️ Same port number can exist for **both TCP and UDP**.

## 9. Open Port vs Closed Port

### Open port

-   A program is **listening**
-   Can accept connections

### Closed port

-   No service is listening
-   Connection rejected

Security note:

>   Fewer open ports = smaller attack surface

## 10. How to Check Ports (Basic)

### macOS / Linux

```
lsof -i :80
netstat -an | grep LISTEN
ss -lnt
```

### Windows

```
netstat -ano
```

## 11. Port in URLs

Sometimes ports appear in URLs.

```
http://localhost:3000
http://192.168.1.10:8080
```

-   `http` default → port **80**
-   `https` default → port **443**
-   Other ports must be specified explicitly

## 12. Common Beginner Mistakes ❌

1.  ❌ Confusing port with IP
2.  ❌ Assuming one port can serve multiple programs
3.  ❌ Forgetting firewall blocks ports
4.  ❌ Using privileged ports without permission
5.  ❌ Exposing services directly to the internet

## 13. One-Sentence Summary

>   A port is a numbered door on a computer that allows multiple network services to share the same IP address safely.