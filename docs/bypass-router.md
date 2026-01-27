# Bypass Router

## 1. What Is a Bypass Router?

A **bypass router** is a secondary router placed **between your main router and the internet** (or inside your network) that:

-   Only processes **specific traffic** (for example, traffic that needs a proxy or special routing)
-   Lets all other traffic **bypass it directly** to the main router or ISP

In simple terms:

>   **Some traffic goes through the “special router”, the rest goes normally.**

## 2. Why Use a Bypass Router?

Beginners usually use a bypass router for these reasons:

### 2.1 Stability

-   Your **main router** stays clean and stable
-   If the bypass router fails, the main network still works

### 2.2 Device Isolation

-   Only certain devices use proxy / special routing
-   Other devices are unaffected

### 2.3 Easy Management

-   No need to install complex plugins on the main router
-   Easier to test, reset, or replace

## 3. Typical Use Cases

| Use Case                   | Explanation                              |
| -------------------------- | ---------------------------------------- |
| Proxy / scientific routing | Only selected traffic goes through proxy |
| Testing OpenWrt / LEDE     | Avoid breaking your main router          |
| Multi-network setup        | Office / lab / IoT separation            |
| Gaming or streaming        | Keep latency-sensitive traffic clean     |

## 4. Basic Network Topologies

### 4.1 Most Common Topology (Recommended)

```
Internet
   │
Main Router (ISP Router)
   │
Bypass Router (OpenWrt / LEDE)
   │
PC / Phone / TV
```

-   Main router handles:
    -   PPPoE / DHCP
    -   Wi-Fi for normal devices
-   Bypass router handles:
    -   Proxy
    -   Policy routing

### 4.2 Parallel Connection (Advanced)

```
Internet
   │
Main Router
 ┌─┴─────────┐
Normal LAN   Bypass Router
               │
          Special Devices
```

This is **not recommended for beginners**.

## 5. What Hardware Do You Need?

### 5.1 Router Requirements

Minimum recommended specs:

-   CPU: Any modern SoC (MT7621, IPQ40xx, etc.)
-   RAM: **128 MB or more**
-   Flash: **16 MB or more**
-   Ethernet ports: At least **2**

Popular beginner-friendly models:

-   GL.iNet MT300N-V2
-   GL.iNet MT1300
-   Xiaomi AX series (with OpenWrt support)

## 6. Software Choices

Most bypass routers run:

-   **OpenWrt**
-   **LEDE** (older fork, concept is the same)

Common routing/proxy solutions:

-   OpenClash
-   PassWall
-   SSR Plus

## 7. Step-by-Step Setup (Beginner Version)

### Step 1: Flash OpenWrt / LEDE

-   Flash firmware via:
    -   Vendor UI
    -   Breed Web
    -   U-Boot (advanced)

After flashing:

-   Access router at `192.168.1.1`

------

### Step 2: Basic Network Configuration

#### WAN Interface

-   Connect bypass router **WAN** to main router **LAN**
-   Set WAN to:
    -   DHCP client (most common)

#### LAN Interface

-   Use a **different subnet** than main router

Example:

-   Main router: `192.168.1.1`
-   Bypass router: `192.168.2.1`

This avoids IP conflicts.

------

### Step 3: Disable Unnecessary Features (Optional)

If main router already handles Wi-Fi:

-   Disable Wi-Fi on bypass router
-   Use wired connection only

------

### Step 4: Install Proxy / Routing Plugin

Using LuCI (Web UI):

1.  Open **System → Software**
2.  Update package lists
3.  Install one of:
    -   OpenClash
    -   PassWall
    -   SSR Plus

------

### Step 5: Configure Policy Routing

This is the **core idea** of a bypass router.

You usually choose one of these modes:

| Mode     | Meaning                        |
| -------- | ------------------------------ |
| GFW list | Only blocked sites use proxy   |
| GeoIP    | Foreign IPs go through proxy   |
| Global   | All traffic goes through proxy |

For beginners:

-   Start with **GFW list** or **GeoIP**

------

### Step 6: Connect Devices

Devices that should use bypass routing:

-   Connect **directly** to the bypass router

Devices that should NOT use it:

-   Connect to the main router

------

## 8. How Traffic Flows (Simple Explanation)

Example: Phone connected to bypass router

1.  Phone sends request
2.  Bypass router checks rules
3.  If matched:
    -   Goes through proxy
4.  If not:
    -   Goes directly to main router
5.  Main router sends traffic to ISP

------

## 9. Common Beginner Mistakes

### 9.1 IP Conflict

-   Main router and bypass router use the same LAN IP
-   Fix: Use different subnets

### 9.2 Wrong Cable Connection

-   WAN ↔ LAN confusion
-   Fix: Main router LAN → Bypass router WAN

### 9.3 DNS Problems

-   Websites open slowly or fail
-   Fix:
    -   Use reliable DNS
    -   Enable fake-IP or redir-host properly

------

## 10. How to Test If It Works

-   Open a blocked website on:
    -   Device connected to bypass router → should work
    -   Device connected to main router → should not
-   Check plugin logs
-   Check public IP differences

------

## 11. When You Should NOT Use a Bypass Router

-   Very limited space or power
-   Only one device needs proxy (client-side app is enough)
-   Ultra-low latency requirements

## 12. Summary

A bypass router is:

-   A **safe**
-   **flexible**
-   **beginner-friendly**

way to introduce advanced routing without breaking your main network.

>   **Main router stays simple.
>   Bypass router does the heavy work.**

