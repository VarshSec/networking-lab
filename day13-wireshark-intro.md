# Day 13 — Wireshark Introduction

## Objective

Learn the basics of packet capture and analysis using Wireshark.

Topics:

- Wireshark interface
- Live packet capture
- Capture interfaces
- Display filters
- TCP traffic
- HTTP traffic
- DNS traffic
- ICMP traffic
- Basic packet inspection

---

# 1. What is Wireshark?

Wireshark is a network protocol analyzer.

It allows us to capture and inspect packets traveling through a network interface.

Basic workflow:

```text
Network Interface
       ↓
Packet Capture
       ↓
Wireshark
       ↓
Filter Traffic
       ↓
Inspect Packets
```

Wireshark is useful for:

- Network troubleshooting
- Protocol analysis
- Learning networking
- Incident investigation
- Security analysis

Only capture traffic on systems and networks you are authorized to monitor.

---

# 2. Check if Wireshark is Installed

In Kali:

```bash
which wireshark
```

Check the version:

```bash
wireshark --version
```

If it is not installed:

```bash
sudo apt update
sudo apt install wireshark
```

Then verify:

```bash
wireshark --version
```

---

# 3. Identify the Active Interface

Run:

```bash
ip -br a
```

Example:

```text
lo       UNKNOWN  127.0.0.1/8
eth0     UP       10.0.2.15/24
```

In this example:

```text
eth0 → active network interface
```

The interface name may be different depending on the VM configuration.

---

# 4. Start Wireshark

Launch:

```bash
wireshark
```

Select the active network interface.

For example:

```text
eth0
```

Start the capture.

---

# 5. Generate Traffic

While capturing, generate some normal traffic inside Kali.

For example:

```bash
ping -c 4 8.8.8.8
```

DNS:

```bash
dig google.com
```

HTTP:

```bash
curl -v http://example.com
```

You can also browse to a website from Kali.

Capture traffic for approximately 60 seconds.

Then stop the capture.

---

# 6. Wireshark Interface

The main Wireshark interface contains three important areas.

```text
+--------------------------------------+
| Packet List                          |
+--------------------------------------+
| Packet Details                       |
+--------------------------------------+
| Packet Bytes                         |
+--------------------------------------+
```

## Packet List

Shows captured packets.

Typical columns include:

- No.
- Time
- Source
- Destination
- Protocol
- Length
- Info

## Packet Details

Shows protocol fields inside the selected packet.

## Packet Bytes

Shows the raw packet bytes in hexadecimal and ASCII representation.

---

# 7. Display Filters

Display filters allow us to focus on specific traffic.

Enter a filter in Wireshark's filter bar.

Important distinction:

```text
Capture filter
    ↓
Controls what gets captured

Display filter
    ↓
Controls what is displayed after capture
```

---

# 8. Filter 1 — HTTP

```text
http
```

### What it does

Displays packets that Wireshark identifies as HTTP traffic.

Useful for inspecting:

- HTTP requests
- HTTP responses
- Methods
- Status codes
- Headers

Example:

```text
GET / HTTP/1.1
```

---

# 9. Filter 2 — TCP

```text
tcp
```

### What it does

Displays TCP packets.

Useful for examining:

- TCP connections
- SYN
- SYN-ACK
- ACK
- TCP flags
- Sequence numbers
- Acknowledgment numbers

For the TCP three-way handshake, look for:

```text
SYN
SYN-ACK
ACK
```

---

# 10. Filter 3 — tcp.port == 80

```text
tcp.port == 80
```

### What it does

Displays TCP traffic where either the source or destination TCP port is 80.

Port 80 is commonly associated with HTTP.

This is useful for examining traffic associated with HTTP's traditional port.

---

# 11. Filter 4 — DNS

```text
dns
```

### What it does

Displays DNS packets.

Useful for observing:

- DNS queries
- DNS responses
- Domain names
- DNS record information
- Query/response relationships

Example concept:

```text
Client
  |
  | DNS query: google.com
  ↓
DNS server
  |
  | DNS response: IP address
  ↓
Client
```

---

# 12. Filter 5 — ICMP

```text
icmp
```

### What it does

Displays ICMP traffic.

For example, when running:

```bash
ping -c 4 8.8.8.8
```

you can inspect ICMP Echo Request and Echo Reply packets.

Conceptually:

```text
Client
  |
  | Echo Request
  ↓
Server
  |
  | Echo Reply
  ↓
Client
```

---

# 13. Five Filters Used

| Filter | What it does |
|---|---|
| `http` | Displays HTTP traffic |
| `tcp` | Displays TCP packets |
| `tcp.port == 80` | Displays TCP traffic involving port 80 |
| `dns` | Displays DNS traffic |
| `icmp` | Displays ICMP traffic |

Quick reference:

```text
http             → HTTP
tcp              → TCP
tcp.port == 80   → TCP port 80
dns              → DNS
icmp             → ICMP
```

---

# 14. TCP Handshake Exercise

Start a capture.

Then create a TCP connection in an authorized lab.

For example, use Netcat:

Terminal 1:

```bash
nc -lvnp 4444
```

Terminal 2:

```bash
nc <kali-ip> 4444
```

In Wireshark use:

```text
tcp
```

Look for:

```text
[SYN]
[SYN, ACK]
[ACK]
```

The sequence is:

```text
Client → SYN → Server
Client ← SYN-ACK ← Server
Client → ACK → Server
```

---

# 15. HTTP Exercise

Run:

```bash
curl http://example.com
```

Then use:

```text
http
```

in Wireshark.

If the traffic is identified as HTTP, inspect:

- Request method
- Request URI
- Host
- Response status
- Headers
- Response data

Note that modern websites commonly use HTTPS, so plain `http` traffic may be limited or redirected.

---

# 16. DNS Exercise

Run:

```bash
dig google.com
```

Then use:

```text
dns
```

Inspect:

- Source
- Destination
- Query name
- Query type
- Response
- Returned address records

---

# 17. ICMP Exercise

Run:

```bash
ping -c 4 8.8.8.8
```

Then filter:

```text
icmp
```

Look for:

```text
Echo (ping) request
Echo (ping) reply
```

---

# 18. 60-Second Capture Lab

## Task

Capture approximately 60 seconds of normal traffic while browsing from Kali.

### Steps

1. Open Wireshark.
2. Select the active interface.
3. Start capture.
4. Browse to an authorized website.
5. Run:

```bash
dig google.com
```

6. Run:

```bash
ping -c 4 8.8.8.8
```

7. Stop the capture after approximately 60 seconds.
8. Apply the five filters.
9. Record what you observed.

---

# 19. Capture Notes

Record:

```text
Interface:
Capture duration:
Approximate packet count:
```

### HTTP

```text
Observed:
```

### TCP

```text
Observed:
```

### TCP port 80

```text
Observed:
```

### DNS

```text
Observed:
```

### ICMP

```text
Observed:
```

---

# 20. Suspicious Traffic — Basic Checklist

Unusual traffic does not automatically mean malicious activity.

When analyzing packets, ask:

```text
WHO?
    ↓
Source and destination

WHERE?
    ↓
IP addresses

WHAT?
    ↓
Protocol

WHICH PORT?
    ↓
Source/destination ports

WHEN?
    ↓
Timing and frequency

HOW MUCH?
    ↓
Packet/data volume

WHAT CONTENT?
    ↓
Headers, DNS queries, payloads where visible

IS IT EXPECTED?
    ↓
Context
```

Examples of things worth investigating:

- Unexpected destinations
- Unusual ports
- Repeated outbound connections
- Unexpected DNS queries
- Large outbound transfers
- Port scanning patterns
- Cleartext sensitive information
- Repeated periodic connections

These are investigation indicators, not proof of malicious activity.

---

# 21. Useful Filters

Additional filters to learn:

```text
ip.addr == 192.168.1.10
```

Shows packets involving an IP.

```text
ip.src == 192.168.1.10
```

Shows packets originating from an IP.

```text
ip.dst == 192.168.1.10
```

Shows packets going to an IP.

```text
tcp.port == 443
```

Shows TCP traffic involving port 443.

```text
udp
```

Shows UDP packets.

---

# 22. GitHub Documentation

Create a directory:

```bash
mkdir -p pcaps
```

Do not commit the actual packet capture if your goal is only to document the exercise.

Create:

```bash
nano pcaps/README.md
```

Example:

```markdown
# Packet Capture Exercises

## Day 13 — Wireshark Intro

### Capture

- Tool: Wireshark
- Interface: eth0
- Duration: approximately 60 seconds
- Environment: Kali Linux VM
- Activity: Web browsing, DNS lookup, ICMP ping

### Traffic Observed

- HTTP
- TCP
- DNS
- ICMP

### Filters Used

```text
http
tcp
tcp.port == 80
dns
icmp
```

### Notes

The capture was used to understand how application traffic is represented as packets and how display filters can isolate specific protocols.

No actual `.pcap` file is stored in this repository.
```

---

# 23. GitHub Commands

After creating the documentation:

```bash
git add day13-wireshark-intro.md pcaps/README.md
```

Commit:

```bash
git commit -m "Day 13: Wireshark introduction"
```

Push:

```bash
git push
```

---

# 24. Interview Questions

### What is Wireshark?

Wireshark is a network protocol analyzer used to capture and inspect network packets.

### What is a display filter?

A display filter controls which packets are shown from an existing capture.

### What does `tcp` do?

It displays TCP packets.

### What does `tcp.port == 80` do?

It displays TCP traffic where the source or destination port is 80.

### What does `dns` do?

It displays DNS traffic.

### What does `icmp` do?

It displays ICMP traffic such as ping requests and replies.

---

# 25. Progress

- [x] Installed/checked Wireshark
- [x] Learned Wireshark interface
- [x] Learned packet capture basics
- [x] Learned display filters
- [x] Learned HTTP filter
- [x] Learned TCP filter
- [x] Learned TCP port filter
- [x] Learned DNS filter
- [x] Learned ICMP filter
- [ ] Complete 60-second capture
- [ ] Complete TryHackMe Wireshark Tasks 1–4
- [ ] Document capture in `pcaps/README.md`

---

# Day 13 Summary

```text
Wireshark
    ↓
Capture packets
    ↓
Apply filters
    ↓
Identify protocols
    ↓
Inspect packet details
    ↓
Understand network behavior
```

Five core filters:

```text
http
tcp
tcp.port == 80
dns
icmp
```

The key learning goal is not just memorizing filters, but understanding how network protocols appear in real packet captures.
