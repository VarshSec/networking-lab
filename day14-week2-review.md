# Day 14 — Week 2 Review

## Objective

Review Days 8–13 without introducing new theory.

Focus areas:

- IPv4 addressing and subnetting
- TCP/IP fundamentals
- DNS, DHCP and HTTP
- Network devices and topologies
- NAT vs Bridged networking
- Wireshark packet capture and filtering

---

# 1. Review Checklist

## Day 8 — Networking Fundamentals

- [ ] OSI/TCP-IP mental model
- [ ] IP address vs MAC address
- [ ] Subnet mask
- [ ] Default gateway
- [ ] ARP
- [ ] DHCP
- [ ] DNS
- [ ] TCP vs UDP
- [ ] Switch vs router vs firewall
- [ ] NAT

## Day 9 — Subnetting

- [ ] IPv4 is 32 bits
- [ ] CIDR notation
- [ ] Network vs host portion
- [ ] Private IPv4 ranges
- [ ] `/24` → four `/26` subnets
- [ ] Network address
- [ ] Broadcast address
- [ ] Usable host range
- [ ] Block size

### Day 9 Reconstruction Exercise

Without looking at the old notes, subnet:

```text
192.168.1.0/24
```

into four equal subnets.

Write your answer first:

| Subnet | Network | First Host | Last Host | Broadcast |
|---|---|---|---|---|
| 1 | | | | |
| 2 | | | | |
| 3 | | | | |
| 4 | | | | |

Then compare your result with Day 9.

### Error Check

- [ ] Correct prefix length
- [ ] Correct subnet mask
- [ ] Correct block size
- [ ] Correct network addresses
- [ ] Correct broadcast addresses
- [ ] Correct usable ranges

---

# 2. Day 10 — TCP, UDP & Ports

Review:

```text
TCP
 ↓
Connection-oriented
 ↓
Reliable / ordered
 ↓
ACKs / retransmission
```

```text
UDP
 ↓
Connectionless
 ↓
Lower overhead
 ↓
No TCP-style reliability
```

TCP handshake:

```text
Client                 Server

   | ---- SYN --------> |
   | <--- SYN-ACK ----- |
   | ---- ACK --------> |
   |                    |
   | Connection Ready   |
```

Common ports:

```text
21   FTP
22   SSH
23   Telnet
25   SMTP
53   DNS
80   HTTP
443  HTTPS
3389 RDP
```

---

# 3. Day 11 — DNS, DHCP & HTTP

DNS:

```text
Domain
  ↓
DNS resolution
  ↓
IP address
```

DHCP:

```text
Discover
   ↓
Offer
   ↓
Request
   ↓
Acknowledge
```

HTTP:

```text
Request
   ↓
Server
   ↓
Response
```

HTTP request structure:

```text
Request Line
     ↓
Headers
     ↓
Blank Line
     ↓
Optional Body
```

HTTP response structure:

```text
Status Line
     ↓
Headers
     ↓
Blank Line
     ↓
Response Body
```

Useful commands:

```bash
nslookup google.com
dig google.com
curl -v http://example.com
```

---

# 4. Day 12 — Devices, Topologies & VirtualBox

Network devices:

```text
Router
 ↓
Connects different IP networks

Switch
 ↓
Connects devices within a LAN

Firewall
 ↓
Controls network traffic
```

Topologies:

```text
Star → Central device
Mesh → Multiple paths
Bus  → Shared backbone
```

VirtualBox:

```text
NAT
 ↓
Kali → VirtualBox NAT → Host → Network
```

```text
Bridged
 ↓
Kali → Physical LAN
```

Review your own observed IP addresses from the Day 12 practical.

---

# 5. Day 13 — Wireshark

Review the packet-analysis workflow:

```text
Capture
   ↓
Filter
   ↓
Select packet
   ↓
Inspect protocol fields
   ↓
Interpret traffic
```

Five core filters:

```text
http
tcp
tcp.port == 80
dns
icmp
```

What they show:

| Filter | Purpose |
|---|---|
| `http` | HTTP traffic |
| `tcp` | TCP packets |
| `tcp.port == 80` | TCP traffic involving port 80 |
| `dns` | DNS traffic |
| `icmp` | ICMP traffic |

TCP handshake in Wireshark:

```text
[SYN]
[SYN, ACK]
[ACK]
```

---

# 6. Practical Review

## Subnetting

Complete the Day 9 reconstruction before checking the old notes.

Result:

```text
My result:
```

Errors found:

```text
```

What caused the error?

```text
```

---

## Wireshark

Complete TryHackMe Wireshark — The Basics Tasks 5–7.

Record:

```text
Sample/capture analyzed:
Important packets observed:
Filters used:
Main observation:
```

---

# 7. Confidence Rating

Rate yourself honestly from **1–10**.

| Area | Confidence (1–10) |
|---|---:|
| Subnetting | |
| TCP/IP | |
| Wireshark | |

### My strongest area

```text
```

### My weakest area

```text
```

### Why is it weak?

```text
```

### Month 2 revisit area

```text
```

---

# 8. Week 2 Retrospective

### What I can now explain without notes

```text
```

### What I can perform practically

```text
```

### What I still need to practice

```text
```

### Biggest improvement during Week 2

```text
```

### One thing I want to be able to do by the end of Month 2

```text
```

---

# 9. Week 2 Mental Map

```text
IPv4
  ↓
Subnet
  ↓
Gateway
  ↓
Routing
  ↓
TCP / UDP
  ↓
Ports
  ↓
DNS / DHCP
  ↓
HTTP / HTTPS
  ↓
Network Devices
  ↓
NAT / Bridged
  ↓
Wireshark
  ↓
Packet Analysis
```

---

# 10. Week 2 Completion

- [ ] Reviewed Days 8–13
- [ ] Recreated Day 9 subnetting from scratch
- [ ] Compared subnetting result with previous work
- [ ] Completed TryHackMe Wireshark Tasks 5–7
- [ ] Finalized `networking-lab` Week 2 structure
- [ ] Added `NETWORK-DIAGRAM.md`
- [ ] Added `/pcaps/README.md`
- [ ] Rated subnetting confidence
- [ ] Rated TCP/IP confidence
- [ ] Rated Wireshark confidence
- [ ] Selected weakest area for Month 2

---

# Week 2 Status

**No new theory today.**

The purpose of Day 14 is retrieval practice, error checking, practical completion, and documentation.
