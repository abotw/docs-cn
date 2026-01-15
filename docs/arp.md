# ARP

**(What ARP is, how it works, and how to use ARP commands)**

------

## 1. Why ARP Exists (The Core Problem)

On a local network (LAN):

-   **IP address** → used by software (Layer 3)
-   **MAC address** → used by network hardware (Layer 2)

👉 **Ethernet does NOT understand IP addresses**
👉 **IP packets must be delivered using MAC addresses**

So the question is:

>   If I know a device’s IP address, how do I find its MAC address?

That is exactly what **ARP** does.

------

## 2. What Is ARP?

**ARP (Address Resolution Protocol)** maps:

```
IP address  →  MAC address
```

Example:

```
192.168.1.1 → aa:bb:cc:dd:ee:ff
```

It works **only inside a local network** (same subnet).

------

## 3. ARP Works at Which Layer?

| Layer   | Name      | ARP Role           |
| ------- | --------- | ------------------ |
| Layer 2 | Data Link | Uses MAC addresses |
| Layer 3 | Network   | Uses IP addresses  |

🔹 ARP is **between Layer 2 and Layer 3**
🔹 It connects IP logic with Ethernet reality

------

## 4. How ARP Works (Step by Step)

Assume:

-   Your computer: `192.168.1.10`
-   Router: `192.168.1.1`
-   You want to send data to the router

### Step 1: Check ARP Cache

Your system checks:

```
Do I already know the MAC for 192.168.1.1?
```

-   Yes → send packet directly
-   No → continue to Step 2

------

### Step 2: Send ARP Request (Broadcast)

Your computer broadcasts:

```
Who has 192.168.1.1?
Tell 192.168.1.10
```

-   Destination MAC: `ff:ff:ff:ff:ff:ff`
-   Everyone in the LAN receives it

------

### Step 3: ARP Reply (Unicast)

The router replies:

```
192.168.1.1 is at aa:bb:cc:dd:ee:ff
```

------

### Step 4: Save to ARP Cache

Your system stores:

```
192.168.1.1 → aa:bb:cc:dd:ee:ff
```

Next time, no broadcast is needed.

------

## 5. What Is the ARP Cache?

The **ARP cache** is a temporary table in memory:

| IP Address  | MAC Address       | Interface |
| ----------- | ----------------- | --------- |
| 192.168.1.1 | aa:bb:cc:dd:ee:ff | eth0      |

-   Entries **expire automatically**
-   Prevents repeated broadcasts
-   Speeds up communication

------

## 6. ARP-Related Commands (Linux)

### 6.1 `ip neigh` (Recommended, Modern)

```bash
ip neigh
```

Example output:

```
192.168.1.1 dev eth0 lladdr aa:bb:cc:dd:ee:ff REACHABLE
```

Fields:

-   `dev` → network interface
-   `lladdr` → MAC address
-   `REACHABLE / STALE / DELAY` → state

🔹 This is the **modern replacement for `arp`**

### 6.2 `arp` (Legacy but Still Common)

```bash
arp -n
```

Example:

```
Address          HWtype  HWaddress           Iface
192.168.1.1      ether   aa:bb:cc:dd:ee:ff   eth0
```

Options:

-   `-n` → **n**umeric output (no DNS lookup)

### 6.3 Add a Static ARP Entry (Rare but Useful)

```bash
sudo arp -s 192.168.1.100 00:11:22:33:44:55
```

Meaning:

-   Manually bind IP → MAC
-   Entry does **not expire**

⚠️ Be careful: wrong entries break networking

------

### 6.4 Delete an ARP Entry

```bash
sudo arp -d 192.168.1.1
```

Or using `ip`:

```bash
sudo ip neigh del 192.168.1.1 dev eth0
```

------

## 7. Trigger ARP Manually

### Using `ping`

```bash
ping 192.168.1.1
```

-   If MAC unknown → ARP request happens automatically
-   After ping, check cache again

------

## 8. ARP States Explained

| State     | Meaning                 |
| --------- | ----------------------- |
| REACHABLE | Recently confirmed      |
| STALE     | Old but usable          |
| DELAY     | Checking if still valid |
| FAILED    | No response             |

Seen via:

```bash
ip neigh
```

------

## 9. Common ARP Use Cases

### 1️⃣ Debug network connectivity

“Why can’t I reach the gateway?”

### 2️⃣ Find devices in local network

“Who is using this IP?”

### 3️⃣ Understand network attacks

ARP spoofing / ARP poisoning

------

## 10. ARP Security (Beginner Awareness)

ARP has **no authentication**.

This allows attacks like:

-   **ARP Spoofing**
-   **Man-in-the-Middle**

Example idea:

>   Attacker lies: “I am the router”

Defenses (advanced topics):

-   Static ARP
-   DHCP snooping
-   Dynamic ARP Inspection (DAI)

------

## 11. Quick Summary

-   ARP maps **IP → MAC**
-   Works only in **local networks**
-   Uses **broadcast requests**
-   Results are stored in **ARP cache**
-   Key commands:
    -   `ip neigh`
    -   `arp -n`
    -   `arp -s`
    -   `arp -d`