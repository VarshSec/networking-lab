# Day 09 — IPv4 Addressing & Subnetting

## Objective

Understand IPv4 addressing, subnet masks, CIDR notation, private vs public IP addresses, and manually divide a `/24` network into four equal subnets.

---

# 1. IPv4 Addressing

IPv4 addresses are 32-bit addresses normally written as four decimal octets.

Example:

```text
192.168.1.10
```

Each octet represents 8 bits:

```text
192 . 168 . 1 . 10
 8     8     8    8 bits
```

Total:

```text
8 + 8 + 8 + 8 = 32 bits
```

---

# 2. Network and Host Portions

An IPv4 address contains a network portion and a host portion.

The subnet mask or CIDR prefix determines where the network portion ends.

Example:

```text
192.168.1.10/24
```

`/24` means:

```text
First 24 bits → Network
Remaining 8 bits → Host
```

Subnet mask:

```text
255.255.255.0
```

---

# 3. CIDR Notation

CIDR stands for Classless Inter-Domain Routing.

Examples:

```text
/8
/16
/24
/25
/26
/27
/28
```

The number represents how many bits belong to the network prefix.

For example:

```text
/24 = 255.255.255.0
```

---

# 4. Private IPv4 Address Ranges

| Range | CIDR |
|---|---|
| 10.0.0.0 – 10.255.255.255 | 10.0.0.0/8 |
| 172.16.0.0 – 172.31.255.255 | 172.16.0.0/12 |
| 192.168.0.0 – 192.168.255.255 | 192.168.0.0/16 |

Private addresses are commonly used inside local networks.

Examples:

```text
192.168.1.20
10.0.0.5
172.16.10.20
```

Public IP addresses are generally globally routable addresses used on the public Internet.

---

# 5. Subnetting

Subnetting divides one network into smaller networks.

Starting network:

```text
192.168.1.0/24
```

Goal:

```text
4 equal subnets
```

---

# 6. Step 1 — Determine Required Bits

We need 4 subnets.

Formula:

```text
2^n = number of subnets
```

For 4 subnets:

```text
2^2 = 4
```

Therefore, borrow 2 host bits.

Original prefix:

```text
/24
```

New prefix:

```text
/24 + 2 = /26
```

---

# 7. Step 2 — Determine Subnet Mask

A `/26` subnet mask is:

```text
255.255.255.192
```

Binary:

```text
11111111.11111111.11111111.11000000
```

The final octet is:

```text
128 + 64 = 192
```

Therefore:

```text
255.255.255.192
```

---

# 8. Step 3 — Determine Block Size

Block size:

```text
256 - subnet mask value
```

For `/26`:

```text
256 - 192 = 64
```

Therefore, subnet addresses increase by 64:

```text
0
64
128
192
```

---

# 9. Address Capacity

Each `/26` subnet contains:

```text
2^(32-26)
= 2^6
= 64 total addresses
```

Usable host addresses:

```text
64 - 2 = 62
```

Two addresses are reserved for:

- Network address
- Broadcast address

---

# 10. Four Equal Subnets

## Subnet 1

Network:

```text
192.168.1.0/26
```

Broadcast:

```text
192.168.1.63
```

Usable range:

```text
192.168.1.1 - 192.168.1.62
```

Subnet mask:

```text
255.255.255.192
```

---

## Subnet 2

Network:

```text
192.168.1.64/26
```

Broadcast:

```text
192.168.1.127
```

Usable range:

```text
192.168.1.65 - 192.168.1.126
```

Subnet mask:

```text
255.255.255.192
```

---

## Subnet 3

Network:

```text
192.168.1.128/26
```

Broadcast:

```text
192.168.1.191
```

Usable range:

```text
192.168.1.129 - 192.168.1.190
```

Subnet mask:

```text
255.255.255.192
```

---

## Subnet 4

Network:

```text
192.168.1.192/26
```

Broadcast:

```text
192.168.1.255
```

Usable range:

```text
192.168.1.193 - 192.168.1.254
```

Subnet mask:

```text
255.255.255.192
```

---

# 11. Complete Subnetting Table

| Subnet | Network Address | Broadcast Address | Usable Range | Subnet Mask |
|---|---|---|---|---|
| 1 | 192.168.1.0/26 | 192.168.1.63 | 192.168.1.1 – 192.168.1.62 | 255.255.255.192 |
| 2 | 192.168.1.64/26 | 192.168.1.127 | 192.168.1.65 – 192.168.1.126 | 255.255.255.192 |
| 3 | 192.168.1.128/26 | 192.168.1.191 | 192.168.1.129 – 192.168.1.190 | 255.255.255.192 |
| 4 | 192.168.1.192/26 | 192.168.1.255 | 192.168.1.193 – 192.168.1.254 | 255.255.255.192 |

---

# 12. Visual Representation

```text
192.168.1.0/24
│
├── 192.168.1.0/26
│   ├── Network:   192.168.1.0
│   ├── Hosts:     .1 - .62
│   └── Broadcast: .63
│
├── 192.168.1.64/26
│   ├── Network:   192.168.1.64
│   ├── Hosts:     .65 - .126
│   └── Broadcast: .127
│
├── 192.168.1.128/26
│   ├── Network:   192.168.1.128
│   ├── Hosts:     .129 - .190
│   └── Broadcast: .191
│
└── 192.168.1.192/26
    ├── Network:   192.168.1.192
    ├── Hosts:     .193 - .254
    └── Broadcast: .255
```

---

# 13. Subnetting Formulas

## Number of Subnets

```text
2^borrowed_bits
```

Example:

```text
2^2 = 4 subnets
```

## Addresses per Subnet

```text
2^host_bits
```

For `/26`:

```text
32 - 26 = 6 host bits

2^6 = 64 addresses
```

## Usable Hosts

```text
2^host_bits - 2
```

Therefore:

```text
64 - 2 = 62 usable hosts
```

---

# 14. Quick Subnet Reference

| CIDR | Subnet Mask | Total Addresses | Usable Hosts |
|---|---|---:|---:|
| /24 | 255.255.255.0 | 256 | 254 |
| /25 | 255.255.255.128 | 128 | 126 |
| /26 | 255.255.255.192 | 64 | 62 |
| /27 | 255.255.255.224 | 32 | 30 |
| /28 | 255.255.255.240 | 16 | 14 |
| /29 | 255.255.255.248 | 8 | 6 |
| /30 | 255.255.255.252 | 4 | 2 |

---

# 15. Practical Subnetting Method

When asked to divide a network into equal subnets:

```text
1. Identify original prefix
        ↓
2. Determine required number of subnets
        ↓
3. Calculate borrowed bits
        ↓
4. Find new prefix
        ↓
5. Determine subnet mask
        ↓
6. Calculate block size
        ↓
7. List network addresses
        ↓
8. Find broadcast addresses
        ↓
9. Find usable host ranges
```

Example:

```text
192.168.1.0/24
       ↓
4 subnets
       ↓
2 borrowed bits
       ↓
/26
       ↓
255.255.255.192
       ↓
Block size = 64
       ↓
0, 64, 128, 192
```

---

# 16. Practical Exercises

## Exercise 1 — Private vs Public

Identify whether these addresses are private or public:

```text
192.168.1.10
10.10.10.10
172.20.5.10
8.8.8.8
```

## Exercise 2 — CIDR Conversion

Convert these prefixes into subnet masks:

```text
/24
/25
/26
/27
```

## Exercise 3 — Subnet a /24

For:

```text
192.168.10.0/24
```

create four equal subnets.

## Exercise 4 — Find the Second Subnet

For:

```text
10.0.0.0/24
```

calculate the network address, broadcast address, and usable host range of the second `/26` subnet.

---

# 17. Key Takeaways

```text
IPv4
 ↓
32-bit logical address

Subnet Mask / CIDR
 ↓
Defines network and host portions

Subnetting
 ↓
Divides one network into smaller networks

/24
 ↓
256 total addresses

/26
 ↓
64 total addresses
62 usable hosts

192.168.1.0/24
 ↓
4 equal /26 networks
```

Final four subnets:

```text
192.168.1.0/26
192.168.1.64/26
192.168.1.128/26
192.168.1.192/26
```

Each subnet has:

```text
64 total addresses
62 usable host addresses
255.255.255.192 subnet mask
```

---

# Goal

Build the ability to look at an IPv4 address and CIDR prefix and determine:

- Network address
- Broadcast address
- Usable host range
- Number of subnets
- Number of hosts
- Subnet mask
- Whether an address is private or public

These skills form the foundation for routing, network security, cloud networking, firewall configuration, and cybersecurity labs.
