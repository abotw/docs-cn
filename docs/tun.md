# TUN Mode

## 1. What Is TUN Mode?

**TUN mode** is a networking technique that works at the **IP layer (Layer 3)**.

-   It creates a **virtual network interface**
-   This interface sends and receives **IP packets**
-   Applications think they are using a real network card, but the data is actually handled by software

👉 In short:
**TUN = virtual IP tunnel**

## 2. TUN vs Real Network Interface

| Real NIC (eth0, wlan0)      | TUN Interface (tun0)   |
| --------------------------- | ---------------------- |
| Physical hardware           | Software-only          |
| Sends Ethernet frames       | Sends IP packets       |
| Connected to cables / Wi-Fi | Connected to a program |

Example:

```text
eth0  -> real Ethernet card
wlan0 -> Wi-Fi card
tun0  -> virtual tunnel interface
```

## 3. Why Do We Need TUN Mode?

TUN mode is mainly used to:

-   Build **VPNs**
-   Redirect traffic
-   Simulate networks
-   Implement custom routing or encryption

Typical use cases:

-   OpenVPN (TUN mode)
-   WireGuard
-   Shadowsocks VPN mode
-   Network experiments and labs

------

## 4. How TUN Mode Works (Big Picture)

### Step-by-step idea

1.  The OS creates a **TUN interface** (e.g. `tun0`)
2.  Applications send IP packets to `tun0`
3.  The kernel forwards packets to a **user-space program**
4.  The program:
    -   encrypts / modifies / encapsulates packets
    -   sends them over a real network (TCP/UDP)
5.  On the other side:
    -   packets are decoded
    -   injected back into another TUN interface

------

## 5. Simple Diagram

```
Application
    |
    |  IP Packet
    v
  tun0 (virtual)
    |
    |  read() / write()
    v
VPN Program (user space)
    |
    |  encrypted UDP/TCP
    v
Internet
```

------

## 6. TUN vs TAP (Very Important)

| Feature      | TUN          | TAP                |
| ------------ | ------------ | ------------------ |
| OSI Layer    | Layer 3 (IP) | Layer 2 (Ethernet) |
| Data type    | IP packets   | Ethernet frames    |
| Supports ARP | ❌ No         | ✅ Yes              |
| Common use   | VPN routing  | VPN bridging       |

**Memory tip**:

-   **TUN = IP tunnel**
-   **TAP = Ethernet bridge**

------

## 7. What Does a TUN Packet Look Like?

In TUN mode, packets are **pure IP packets**:

```
[ IP Header ][ TCP/UDP ][ Data ]
```

No Ethernet header:

```
❌ No MAC address
❌ No ARP
```

That’s why TUN is lighter and faster for routing-based VPNs.

------

## 8. Example: OpenVPN TUN Mode

When OpenVPN runs in TUN mode:

1.  Creates `tun0`

2.  Assigns an IP (e.g. `10.8.0.2`)

3.  Changes routing table:

    ```bash
    default route -> tun0
    ```

4.  All traffic goes into the tunnel

------

## 9. Check TUN Interface on Linux

### List interfaces

```bash
ip addr
```

Example output:

```text
tun0: <POINTOPOINT,MULTICAST,NOARP,UP>
    inet 10.8.0.2/24
```

### Check routes

```bash
ip route
```

------

## 10. Why TUN Is Preferred for VPNs

Advantages:

-   ✅ Simple
-   ✅ Fast
-   ✅ Works well with routing
-   ✅ No Ethernet overhead
-   ✅ Easy to scale

Disadvantages:

-   ❌ Cannot do LAN broadcast
-   ❌ No Layer 2 protocols

------

## 11. When Should You Use TUN Mode?

Use **TUN mode** when:

-   You only care about IP traffic
-   You want a VPN
-   You don’t need Ethernet bridging
-   You want better performance

Use **TAP mode** when:

-   You need LAN-like behavior
-   You need broadcasts or ARP

------

## 12. Beginner Summary

-   **TUN** creates a **virtual IP interface**
-   Works at **Layer 3**
-   Handles **IP packets only**
-   Commonly used in **VPNs**
-   Faster and simpler than TAP

>   If you remember only one sentence:
>   **TUN mode = software network card for IP packets**

