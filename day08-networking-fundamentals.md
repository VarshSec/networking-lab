# Day 08 — Networking Fundamentals & OSI Model

## Objective

Build a foundation in computer networking by understanding the OSI model, basic network communication, IP addressing, protocols, and packet flow.

---

# 1. OSI Model

The OSI (Open Systems Interconnection) model divides network communication into seven conceptual layers.

```text
┌─────────────────────────────┐
│ L7 — Application            │ → HTTP / DNS / SSH
├─────────────────────────────┤
│ L6 — Presentation           │ → TLS
├─────────────────────────────┤
│ L5 — Session                │ → RPC
├─────────────────────────────┤
│ L4 — Transport              │ → TCP / UDP
├─────────────────────────────┤
│ L3 — Network                │ → IP
├─────────────────────────────┤
│ L2 — Data Link              │ → Ethernet
├─────────────────────────────┤
│ L1 — Physical               │ → Ethernet PHY
└─────────────────────────────┘
```

## Layer 7 — Application

Provides network services used directly by applications.

Examples:
- HTTP
- DNS
- SSH

## Layer 6 — Presentation

Handles data representation, encoding, encryption, and related transformations.

Example:
- TLS

## Layer 5 — Session

Manages communication sessions between applications.

Example:
- RPC

## Layer 4 — Transport

Provides end-to-end communication between applications.

Examples:
- TCP
- UDP

## Layer 3 — Network

Handles logical addressing and routing between networks.

Example:
- IP

## Layer 2 — Data Link

Handles local network communication using frames and MAC addresses.

Example:
- Ethernet

## Layer 1 — Physical

Transmits raw bits through the physical medium.

Examples:
- Ethernet physical layer
- Radio signals

---

# 2. TCP/IP Mental Model

```text
Application
     ↓
TCP / UDP
     ↓
IP
     ↓
Ethernet / Wi-Fi
     ↓
Physical Medium
```

When accessing a website:

```text
Domain
  ↓
DNS
  ↓
IP Address
  ↓
Routing
  ↓
TCP / UDP
  ↓
Port
  ↓
Application Protocol
  ↓
Request / Response
```

---

# 3. Important Networking Concepts

## IP Address

An IP address is a logical address used to identify a network interface and help route traffic.

Example:

```text
192.168.1.20
```

## MAC Address

A MAC address is associated with a network interface and is primarily used for Layer 2 communication on the local network.

Example:

```text
08:00:27:12:34:56
```

Simple distinction:

```text
IP  → Layer 3 → Routing
MAC → Layer 2 → Local network
```

## Subnet Mask

A subnet mask determines which portion of an IP address represents the network and which portion represents the host.

Example:

```text
192.168.1.20/24
```

Equivalent subnet mask:

```text
255.255.255.0
```

## Default Gateway

The default gateway is normally the router used to reach networks outside the local subnet.

## ARP

ARP helps discover the MAC address associated with a known IPv4 address on the local network.

```text
Known IP
   ↓
  ARP
   ↓
MAC Address
```

Useful command:

```bash
ip neigh
```

## DHCP

DHCP automatically provides network configuration such as IP address, subnet mask, default gateway, and DNS server.

DORA:

```text
Discover
   ↓
Offer
   ↓
Request
   ↓
Acknowledge
```

## DNS

DNS translates human-readable domain names into IP addresses.

```text
google.com
     ↓
    DNS
     ↓
IP address
```

Useful commands:

```bash
dig google.com
nslookup google.com
```

---

# 4. TCP vs UDP

## TCP

TCP is connection-oriented and provides reliable, ordered delivery.

Characteristics:
- Connection-oriented
- Reliable
- Ordered
- Uses acknowledgments
- Supports retransmission

Examples:
- HTTPS
- SSH

## UDP

UDP is connectionless and has lower overhead.

Characteristics:
- Connectionless
- Lower overhead
- No TCP-style delivery guarantee
- No TCP-style ordering guarantee

Examples:
- DNS
- DHCP

---

# 5. TCP Three-Way Handshake

```text
Client                    Server

   SYN ───────────────────►

       ◄──────────────── SYN-ACK

   ACK ───────────────────►

       Connection established
```

Memory:

```text
SYN → SYN-ACK → ACK
```

---

# 6. Common Ports

| Port | Service |
|---:|---|
| 21 | FTP |
| 22 | SSH |
| 23 | Telnet |
| 25 | SMTP |
| 53 | DNS |
| 80 | HTTP |
| 443 | HTTPS |
| 3389 | RDP |

---

# 7. Networking Devices

## Switch

A switch primarily connects devices within a LAN and forwards Ethernet frames using MAC addresses.

## Router

A router connects different IP networks and forwards packets based on IP addressing and routing information.

## Firewall

A firewall controls network traffic according to security rules.

---

# 8. NAT

NAT stands for Network Address Translation.

It commonly allows devices using private IP addresses to communicate through a public IP address.

```text
Laptop      192.168.1.10
Phone       192.168.1.11
                 │
                 ▼
                NAT
                 │
                 ▼
             Public IP
                 │
                 ▼
              Internet
```

---

# 9. Practical Commands

```bash
ip a
ip -br a
ip route
ip neigh
ss -tulnp
ping -c 4 8.8.8.8
dig google.com
nslookup google.com
curl -v http://example.com
curl -v https://example.com
```

---

# 10. Wireshark

Wireshark is a network protocol analyzer used to capture and inspect network traffic.

Basic filters:

```text
dns
tcp
udp
icmp
http
tls
tcp.port == 80
tcp.port == 443
```

A packet may contain several protocol layers:

```text
Ethernet
   ↓
IP
   ↓
TCP
   ↓
TLS
   ↓
Application Data
```

---

# 11. Practical Exercises

1. Run `ip -br a` and identify the active interface.
2. Run `ip route` and identify the default gateway.
3. Run `ip neigh` and inspect local IP-to-MAC mappings.
4. Run `ping -c 4 8.8.8.8`.
5. Run `dig google.com`.
6. Open Wireshark and capture traffic while browsing a website.
7. Apply `dns`, `tcp`, `tls`, and `http` filters.

---

# 12. Key Takeaways

```text
OSI
 ↓
Layers describe network communication

IP
 ↓
Logical addressing / routing

MAC
 ↓
Local Layer 2 communication

TCP / UDP
 ↓
Transport communication

Port
 ↓
Application/service endpoint

DNS
 ↓
Domain → IP

DHCP
 ↓
Network configuration

Router
 ↓
Connects networks

Switch
 ↓
Connects devices within a LAN

Wireshark
 ↓
Observe and analyze packets
```

## Goal

Build a strong networking foundation that can later be applied to:

- Network security
- SOC analysis
- Web security
- API security
- Vulnerability assessment
- Packet analysis
- Incident investigation
