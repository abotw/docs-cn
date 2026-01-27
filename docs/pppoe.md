# PPPoE

*(Point-to-Point Protocol over Ethernet)*

## 1. What Is PPPoE?

**PPPoE** stands for:

>   **Point-to-Point Protocol over Ethernet**

It is a network protocol used by many ISPs to:

-   Authenticate users (username & password)
-   Assign IP addresses
-   Manage billing and sessions

In simple terms:

>   **PPPoE lets your ISP know *who you are* before giving you internet access.**

## 2. Why PPPoE Exists

Originally, ISPs used **PPP** over dial-up lines.
When broadband (DSL / fiber) arrived, ISPs needed:

-   User authentication
-   Per-user accounting
-   Session control

Ethernet alone **cannot authenticate users**, so PPP was encapsulated inside Ethernet frames.

That combination is **PPPoE**.

## 3. Where PPPoE Is Used

Common scenarios:

| Access Type    | Uses PPPoE  |
| -------------- | ----------- |
| ADSL           | Yes         |
| VDSL           | Yes         |
| FTTH (fiber)   | Very common |
| Cable modem    | Usually no  |
| Mobile (4G/5G) | No          |

If your ISP gives you:

-   **Username**
-   **Password**

You are almost certainly using **PPPoE**.

## 4. PPPoE in a Home Network

### Typical Home Setup

```
Internet
   │
ISP (PPPoE Server)
   │
ONT / Modem (Bridge Mode)
   │
Router (PPPoE Client)
   │
LAN Devices
```

Key point:

-   **Only one device** performs PPPoE dialing
-   Usually your **main router**

## 5. PPPoE vs DHCP (Important Comparison)

| Feature           | PPPoE           | DHCP           |
| ----------------- | --------------- | -------------- |
| Authentication    | Yes             | No             |
| Username/Password | Required        | Not needed     |
| Session control   | Yes             | No             |
| Overhead          | Slightly higher | Lower          |
| Typical user      | ISP customers   | Local networks |

Think of it like this:

-   **PPPoE** → logging into the internet
-   **DHCP** → getting an IP inside a local network

## 6. How PPPoE Works (Step by Step)

PPPoE has **two phases**:

------

### 6.1 Discovery Phase

Goal: find the ISP’s PPPoE server

Steps:

1.  **PADI** – Client broadcasts: “Any PPPoE server here?”
2.  **PADO** – Server replies
3.  **PADR** – Client requests a session
4.  **PADS** – Server confirms session ID

At this point:

-   A **logical point-to-point link** is created

------

### 6.2 Session Phase

Now real PPP begins:

1.  **LCP** – Link parameters negotiation
2.  **Authentication**
    -   PAP (plain)
    -   CHAP (challenge-response)
3.  **IPCP**
    -   Assign IP address
    -   Set DNS, MTU, etc.

Once complete:

>   Internet access is established

## 7. What Information You Need for PPPoE

From your ISP:

-   PPPoE username
-   PPPoE password

Optional:

-   VLAN ID (common in fiber networks)
-   MTU requirement

## 8. Configuring PPPoE on a Router (Beginner View)

### Required Settings

| Setting      | Value          |
| ------------ | -------------- |
| WAN protocol | PPPoE          |
| Username     | From ISP       |
| Password     | From ISP       |
| MTU          | 1492 (default) |

Why MTU = 1492?

-   Ethernet MTU = 1500
-   PPPoE header = 8 bytes
-   1500 − 8 = 1492

------

### Common WAN Modes Explained

| Mode      | When to use               |
| --------- | ------------------------- |
| PPPoE     | ISP gives account         |
| DHCP      | Cable / shared LAN        |
| Static IP | Enterprise / special plan |
| Bridge    | Modem only, router dials  |

## 9. PPPoE and Modems (Very Important)

### Bridge Mode vs Router Mode

| Modem Mode | Who does PPPoE |
| ---------- | -------------- |
| Bridge     | Your router    |
| Router     | Modem itself   |

Best practice:

>   **Modem in bridge mode + router dials PPPoE**

This avoids:

-   Double NAT
-   Port forwarding issues

## 10. PPPoE in OpenWrt / LEDE (Conceptual)

In OpenWrt:

-   PPPoE is a **WAN interface protocol**
-   `pppd` handles authentication and session

Key files (advanced):

-   `/etc/config/network`
-   `/etc/ppp/`

Beginners should:

-   Use LuCI (web UI)
-   Avoid manual config at first

------

## 11. Common Problems & Causes

### 11.1 PPPoE Dial Fails

Possible reasons:

-   Wrong username/password
-   ISP limits concurrent sessions
-   Wrong VLAN ID
-   ONT not in bridge mode

------

### 11.2 Connected but No Internet

Causes:

-   DNS misconfiguration
-   MTU mismatch
-   Firewall blocking

------

### 11.3 Frequent Disconnects

Causes:

-   Poor fiber / DSL signal
-   ISP timeout policy
-   Router CPU overload

------

## 12. PPPoE Performance Considerations

-   PPPoE adds **CPU overhead**
-   On weak routers:
    -   High-speed PPPoE may bottleneck
-   Hardware acceleration may not apply

Rule of thumb:

-   Gigabit PPPoE needs a **strong CPU**

------

## 13. PPPoE in Multi-Router Setups

### Correct Design

```
ONT (Bridge)
   │
Main Router (PPPoE)
   │
Bypass Router / AP
```

### Wrong Design

```
ONT
 ├─ Router A (PPPoE)
 └─ Router B (PPPoE) ❌
```

Most ISPs allow **only one PPPoE session**.

------

## 14. When You Should Care About PPPoE

You should understand PPPoE if you:

-   Flash OpenWrt / LEDE
-   Build bypass router setups
-   Use bridge mode ONT
-   Debug WAN issues

## 15. Summary

-   PPPoE = PPP + Ethernet
-   Used for ISP authentication
-   Requires username & password
-   Usually handled by the main router
-   Still very common in fiber networks

>   PPPoE is not “old” — it is simply “infrastructure”.