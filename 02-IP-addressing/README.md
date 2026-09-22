
<div align="center">

# 🌐 IP Addressing & Subnetting
### *A Complete Reference Guide for Networking & DevOps*

![Networking](https://img.shields.io/badge/Topic-Networking-blue?style=for-the-badge)
![IPv4](https://img.shields.io/badge/IPv4-Supported-success?style=for-the-badge)
![IPv6](https://img.shields.io/badge/IPv6-Supported-informational?style=for-the-badge)
![CIDR](https://img.shields.io/badge/CIDR-Explained-orange?style=for-the-badge)

</div>

---

## 📑 Table of Contents

1. [🧭 What is an IP Address?](#1--what-is-an-ip-address)
   - 1.1 [How IP Addressing Works](#11-how-ip-addressing-works)
   - 1.2 [The Binary Nature of IP Addresses](#12-the-binary-nature-of-ip-addresses)
2. [🗂️ Types of IP Addresses](#2-️-types-of-ip-addresses)
3. [🏷️ IP Address Classes (IPv4)](#3-️-ip-address-classes-ipv4)
4. [🔒 Private vs Public IP Addresses](#4--private-vs-public-ip-addresses)
5. [✂️ Subnetting Basics](#5-️-subnetting-basics)
   - 5.1 [Understanding Subnet Masks](#51-understanding-subnet-masks)
   - 5.2 [Subnet Mask Calculation](#52-subnet-mask-calculation)
   - 5.3 [Real-World Example (AWS VPC)](#53-real-world-example-aws-vpc)
6. [📐 CIDR Notation](#6--cidr-notation)
   - 6.1 [Why CIDR Was Needed](#61-why-cidr-was-needed)
   - 6.2 [Understanding CIDR Notation](#62-understanding-cidr-notation)
   - 6.3 [CIDR Calculation Examples](#63-cidr-calculation-examples)
   - 6.4 [Supernetting and Aggregation](#64-supernetting-and-aggregation)
7. [💻 Useful Commands](#7--useful-commands)
8. [☁️ DevOps Real-World Use Cases](#8-️-devops-real-world-use-cases)
9. [✅ Key Takeaways](#9--key-takeaways)

---

## 1. 🧭 What is an IP Address?

> An IP address is like a **postal address** for your computer on a network. It helps devices find and communicate with each other.

An **IP (Internet Protocol) address** is a unique numerical label assigned to each device participating in a computer network. It serves two principal functions:

| # | Function | Description |
|:-:|----------|-------------|
| 1️⃣ | **Host or network interface identification** | Uniquely identifies a device |
| 2️⃣ | **Location addressing** | Provides the location of the device in the network |

### 1.1 How IP Addressing Works

When data travels across networks, it's broken into **packets**. Each packet contains:

- 📤 **Source IP address** — Where the packet came from
- 📥 **Destination IP address** — Where the packet is going
- 📦 **Payload** — The actual data being transmitted

> 💡 Routers use IP addresses to determine the best path for packets to reach their destination — think of it as a postal system where each house has a unique address, and the postal service uses these addresses to deliver mail.

### 1.2 The Binary Nature of IP Addresses

IP addresses are actually **binary numbers** that humans read in decimal format for convenience.

**Example:** `192.168.1.1` is actually:

```
11000000.10101000.00000001.00000001
```

Each decimal number (octet) represents **8 bits**, which is why:

| Value | Binary |
|-------|--------|
| Minimum: `0` | `00000000` |
| Maximum: `255` | `11111111` |

> 🎯 Understanding this binary nature is crucial for understanding subnetting and network masks.

# IP Address to Binary Conversion (\(2^n\) Method)

This guide explains how an IPv4 address converts from dot-decimal notation to its 32-bit binary representation using the **base-2 positional value method**.

## Quick Overview

The IP address **192.168.1.1** translates to:

```text
Decimal:     192    .     168    .      1     .      1
Binary:   11000000  .  10101000  .  00000001  .  00000001
```

---

## Understanding the Binary Grid (\(2^n\))

Each of the 4 segments (octets) in an IP address contains **8 bits**. Each bit position represents a power of 2, starting from right (\(2^0\)) to left (\(2^7\)):

| Bit Position | 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Power of 2** | \(2^7\) | \(2^6\) | \(2^5\) | \(2^4\) | \(2^3\) | \(2^2\) | \(2^1\) | \(2^0\) |
| **Decimal Value** | **128** | **64** | **32** | **16** | **8** | **4** | **2** | **1** |

* If a bit is **1**, you **add** that positional value.
* If a bit is **0**, you **skip** it.

---

## Step-by-Step Octet Conversion

### 1. First Octet: `192`
We need to find which powers of 2 add up to 192.
* Can we take 128 (\(2^7\))? Yes (\(192 - 128 = 64\) remaining) \(\rightarrow\) **1**
* Can we take 64 (\(2^6\))? Yes (\(64 - 64 = 0\) remaining) \(\rightarrow\) **1**
* All other positions become **0**.

| \(2^7\) (128) | \(2^6\) (64) | \(2^5\) (32) | \(2^4\) (16) | \(2^3\) (8) | \(2^2\) (4) | \(2^1\) (2) | \(2^0\) (1) |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **1** | **1** | **0** | **0** | **0** | **0** | **0** | **0** |

* **Binary:** `11000000`

---

### 2. Second Octet: `168`
* Can we take 128 (\(2^7\))? Yes (\(168 - 128 = 40\) remaining) \(\rightarrow\) **1**
* Can we take 64 (\(2^6\))? No (too big, 64 > 40) \(\rightarrow\) **0**
* Can we take 32 (\(2^5\))? Yes (\(40 - 32 = 8\) remaining) \(\rightarrow\) **1**
* Can we take 16 (\(2^4\))? No (too big, 16 > 8) \(\rightarrow\) **0**
* Can we take 8 (\(2^3\))? Yes (\(8 - 8 = 0\) remaining) \(\rightarrow\) **1**
* All remaining positions become **0**.

| \(2^7\) (128) | \(2^6\) (64) | \(2^5\) (32) | \(2^4\) (16) | \(2^3\) (8) | \(2^2\) (4) | \(2^1\) (2) | \(2^0\) (1) |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **1** | **0** | **1** | **0** | **1** | **0** | **0** | **0** |

* **Binary:** `10101000`

---

### 3. Third & Fourth Octet: `1`
* The only positional value we need is \(2^0\) (1). All other positions are **0**.

| \(2^7\) (128) | \(2^6\) (64) | \(2^5\) (32) | \(2^4\) (16) | \(2^3\) (8) | \(2^2\) (4) | \(2^1\) (2) | \(2^0\) (1) |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **0** | **0** | **0** | **0** | **0** | **0** | **0** | **1** |

* **Binary:** `00000001`

---

## Final Output

Combining all four 8-bit blocks gives the final string:
`11000000.10101000.00000001.00000001`




---

## 2. 🗂️ Types of IP Addresses

### 🔹 IPv4 *(Most Common)*
- Format: `192.168.1.1`
- 4 numbers separated by dots
- Each number ranges from `0` to `255`
- Example: `172.16.0.10`

### 🔹 IPv6 *(Newer Version)*
- Format: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`
- Designed to replace IPv4 as we run out of addresses
- Example: `fe80::1`

---

## 3. 🏷️ IP Address Classes (IPv4)

| Class | Range | Use Case | Example |
|:-----:|-------|----------|---------|
|  **A** | `1.0.0.0` – `126.255.255.255` | Large networks | `10.0.0.0` |
|  **B** | `128.0.0.0` – `191.255.255.255` | Medium networks | `172.16.0.0` |
|  **C** | `192.0.0.0` – `223.255.255.255` | Small networks | `192.168.1.0` |
|  **D** | `224.0.0.0` – `239.255.255.255` | Multicasting (Streaming/Routing) | `224.0.0.1` |
|  **E** | `240.0.0.0` – `255.255.255.255` | Experimental & Research | `240.0.0.7` |


---

## 4. 🔒 Private vs Public IP Addresses

### 🏠 Private IP Addresses *(Used in your home/office)*

| Range |
|-------|
| `10.0.0.0` – `10.255.255.255` |
| `172.16.0.0` – `172.31.255.255` |
| `192.168.0.0` – `192.168.255.255` |

> **Example:** Your laptop at home might have `192.168.1.5` — this is private and only visible within your home network.

### 🌍 Public IP Addresses

- ✅ Unique across the entire internet
- ✅ Assigned by your ISP
- **Example:** `8.8.8.8` (Google's DNS server)

---
>A subnet is a physical or logical subdivision of a larger network, CIDR is the flexible naming system used to define the size and boundary of that subnet.
>
>> ### 🔍 Quick Comparison: Subnet Mask vs. CIDR
> 
> | Feature | Subnet Mask | CIDR Notation |
> | :--- | :--- | :--- |
> | **Format** | Dotted decimal (e.g., `255.255.255.0`) | Slash prefix (e.g., `/24`) |
> | **Function** | Defines network vs. host bits | Shorthand count of network bits |
> | **Usage** | Legacy system & local network configuration inputs | Modern routing, cloud configurations (AWS/Azure), & firewalls |
![Uploading ip-subnetting-cover.svg…]()


---
## 5. ✂️ Subnetting Basics

**Subnetting** divides a network into smaller networks (subnets). This is fundamental to network design and allows for:

- ⚡ **Efficient IP address allocation** — Don't waste addresses
- 🧩 **Network segmentation** — Separate departments, services, or security zones
- 🚀 **Performance improvement** — Smaller broadcast domains = less network congestion
- 🛡️ **Security** — Isolate sensitive systems from general traffic

### 5.1 Understanding Subnet Masks

A **subnet mask** is a 32-bit number that divides an IP address into network and host portions.

**How it works:**
- Binary `1` bits indicate the **network portion**
- Binary `0` bits indicate the **host portion**

**Example:** IP address `192.168.1.100` with subnet mask `255.255.255.0`

```
IP Address:    192.168.1.100    11000000.10101000.00000001.01100100
Subnet Mask:   255.255.255.0    11111111.11111111.11111111.00000000
                                 ^^^^^^^^^^^^^^^^^^^^^^^^^ ^^^^^^^^
                                 Network portion (24 bits) Host (8 bits)

Network Address:   192.168.1.0    (all host bits are 0)
Broadcast Address: 192.168.1.255  (all host bits are 1)
Usable hosts:      192.168.1.1 to 192.168.1.254
```

### 5.2 Subnet Mask Calculation

- 🧭 **Network portion** — Identifies the specific network
- 🖥️ **Host portion** — Identifies the specific device on that network

**Common subnet masks:**

| Subnet Mask | CIDR | Total Addresses | Usable Hosts |
|-------------|:----:|:----------------:|:-------------:|
| `255.255.255.0` | `/24` | 256 | 254 |
| `255.255.0.0` | `/16` | 65,536 | 65,534 |
| `255.255.255.128` | `/25` | 128 | 126 |
| `255.255.255.192` | `/26` | 64 | 62 |

> ❓ **Why 254 usable and not 256?**
> - The **first address** (all 0s in host portion) = Network address
> - The **last address** (all 1s in host portion) = Broadcast address
> - Both are reserved and cannot be assigned to hosts

### 5.3 Real-World Example (AWS VPC)

**Scenario:** You're setting up an AWS VPC

```
VPC CIDR: 10.0.0.0/16
├── Public Subnet:    10.0.1.0/24   (for web servers)
├── Private Subnet:   10.0.2.0/24   (for app servers)
└── Database Subnet:  10.0.3.0/24   (for databases)
```

---

## 6. 📐 CIDR Notation

**CIDR** (Classless Inter-Domain Routing) was introduced in **1993** to replace the older classful addressing system. It provides a more flexible way to allocate IP addresses.

### 6.1 Why CIDR Was Needed

**❌ Problem with Classful Addressing:**
- Class C gave only 254 hosts (too small for many organizations)
- Class B gave 65,534 hosts (too large, wasted addresses)
- No middle ground, leading to inefficient address allocation

**✅ CIDR Solution:**
> Allows network masks of any length, not just 8, 16, or 24 bits.

### 6.2 Understanding CIDR Notation

The `/` notation indicates how many bits are used for the network portion.

| CIDR | Meaning |
|:----:|---------|
| `192.168.1.0/24` | First 24 bits are network, last 8 bits are for hosts |
| `/24` | 256 addresses (254 usable) |
| `/16` | 65,536 addresses (65,534 usable) |
| `/32` | Single IP address (all 32 bits for network, 0 for host) |
| `/0` | All IP addresses (used for default routes) |

### 6.3 CIDR Calculation Examples

<details open>
<summary><strong>📍 Example 1: <code>10.0.0.0/8</code></strong></summary>

```
Network bits: 8
Host bits: 32 - 8 = 24
Number of addresses: 2^24 = 16,777,216
Subnet mask: 255.0.0.0
Range: 10.0.0.0 to 10.255.255.255
```
</details>

<details open>
<summary><strong>📍 Example 2: <code>172.16.0.0/12</code></strong></summary>

```
Network bits: 12
Host bits: 32 - 12 = 20
Number of addresses: 2^20 = 1,048,576
Subnet mask: 255.240.0.0
Range: 172.16.0.0 to 172.31.255.255
```
</details>

<details open>
<summary><strong>📍 Example 3: <code>192.168.1.0/26</code></strong></summary>

```
Network bits: 26
Host bits: 32 - 26 = 6
Number of addresses: 2^6 = 64
Usable hosts: 64 - 2 = 62
Subnet mask: 255.255.255.192
Range: 192.168.1.0 to 192.168.1.63
```
</details>

### 6.4 Supernetting and Aggregation

CIDR also enables **route aggregation** (supernetting), where multiple networks can be summarized into a single route:

```
Instead of advertising:
- 192.168.1.0/24
- 192.168.2.0/24
- 192.168.3.0/24
- 192.168.4.0/24

Advertise single route:
- 192.168.0.0/22   (covers all four networks)
```

> 🚀 This reduces routing table size and improves routing efficiency.

---

## 7. 💻 Useful Commands

### 🔍 Check your IP address

```bash
# On Linux/Mac
ip addr show
# or
ifconfig

# On Windows
ipconfig
```

### 📡 Check if a host is reachable

```bash
ping 8.8.8.8
```

---

## 8. ☁️ DevOps Real-World Use Cases

| # | Use Case | Description |
|:-:|----------|-------------|
| 1 | 🏗️ **Cloud VPC Design** | Creating subnets for different tiers (web, app, database) |
| 2 | 🐳 **Container Networks** | Docker assigns IP addresses from a subnet (usually `172.17.0.0/16`) |
| 3 | ☸️ **Kubernetes Pods** | Each pod gets its own IP address |
| 4 | 🛡️ **Security Groups** | Allow traffic from specific IP ranges (e.g., `10.0.1.0/24`) |

---

## 9. ✅ Key Takeaways

- ✅ IP addresses identify devices on a network
- ✅ Private IPs are for internal networks
- ✅ Public IPs are for internet communication
- ✅ Subnetting helps organize and secure networks
- ✅ CIDR notation is used everywhere in cloud/DevOps

---

<div align="center">

**📘 Made for quick reference in Networking & DevOps workflows**

</div>
