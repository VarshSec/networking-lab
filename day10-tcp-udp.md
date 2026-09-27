# Day 10 — TCP, UDP, Ports & Three-Way Handshake

## Objective

Understand the Transport Layer and how TCP and UDP provide communication between applications.

Topics covered:

- TCP vs UDP
- Port numbers
- TCP connection establishment
- Three-way handshake
- SYN
- SYN-ACK
- ACK
- Common network ports
- Netcat practical

---

# 1. Transport Layer

The Transport Layer is responsible for communication between applications running on different devices.

The main transport protocols are:

- TCP
- UDP

A simple mental model:

```text
Application
     ↓
Transport
     ↓
Network
     ↓
Data Link
     ↓
Physical
```

TCP and UDP operate at Layer 4 of the OSI model.

---

# 2. TCP

TCP stands for:

**Transmission Control Protocol**

TCP is connection-oriented.

Before application data is exchanged, TCP establishes a connection between the two endpoints.

TCP provides:

- Reliable delivery
- Ordered data
- Acknowledgments
- Retransmission
- Flow control
- Connection management

Examples where TCP is commonly used:

- SSH
- HTTP
- HTTPS
- FTP

---

# 3. UDP

UDP stands for:

**User Datagram Protocol**

UDP is connectionless.

It does not establish a TCP-style connection before sending data.

UDP has lower overhead than TCP but does not provide TCP's built-in reliability and ordering mechanisms.

Examples where UDP is commonly used:

- DNS
- DHCP
- VoIP
- Streaming
- Online gaming

The exact protocol used depends on the application and implementation.

---

# 4. TCP vs UDP

| Feature | TCP | UDP |
|---|---|---|
| Connection | Connection-oriented | Connectionless |
| Reliability | Yes | No TCP-style guarantee |
| Ordering | Yes | No guarantee |
| ACKs | Yes | No TCP-style ACK |
| Retransmission | Yes | No TCP-style retransmission |
| Overhead | Higher | Lower |
| Typical use | SSH, HTTP/HTTPS | DNS, DHCP, VoIP |

---

# 5. Ports

An IP address identifies a host/interface.

A port identifies a logical endpoint associated with an application or service.

Example:

```text
192.168.1.20:443
       │      │
       │      └── Port
       │
       └──────── IP address
```

Think:

```text
IP address → Which device?
Port → Which service/application?
```

---

# 6. Common Ports

| Port | Service | Purpose |
|---:|---|---|
| 21 | FTP | File transfer |
| 22 | SSH | Secure remote access |
| 23 | Telnet | Remote terminal access |
| 25 | SMTP | Email transmission |
| 53 | DNS | Domain name resolution |
| 80 | HTTP | Web traffic |
| 443 | HTTPS | Secure web traffic |
| 3389 | RDP | Remote Desktop |

Quick memory:

```text
21 → FTP
22 → SSH
23 → Telnet
25 → SMTP
53 → DNS
80 → HTTP
443 → HTTPS
3389 → RDP
```

---

# 7. TCP Three-Way Handshake

Before normal TCP data exchange begins, TCP establishes the connection using a three-way handshake.

The three messages are:

```text
SYN
SYN-ACK
ACK
```

## Step 1 — SYN

The client sends a SYN packet to the server.

```text
Client
  |
  | -------- SYN -------->
  |
Server
```

The client is essentially requesting to establish a TCP connection.

## Step 2 — SYN-ACK

The server responds with SYN-ACK.

```text
Client
  |
  | -------- SYN -------->
  |
  | <------ SYN-ACK ------
  |
Server
```

The server acknowledges the client's SYN and also sends its own synchronization request.

## Step 3 — ACK

The client sends an ACK back to the server.

```text
Client
  |
  | -------- SYN -------->
  | <------ SYN-ACK ------
  | -------- ACK -------->
  |
Server
```

The TCP connection is now established.

---

# 8. Complete Handshake Diagram

```text
              TCP THREE-WAY HANDSHAKE

Client                                      Server
  |                                           |
  | ------------ SYN -----------------------> |
  |                                           |
  | <----------- SYN-ACK -------------------- |
  |                                           |
  | ------------ ACK -----------------------> |
  |                                           |
  |          CONNECTION ESTABLISHED           |
  |                                           |
  | <========= DATA EXCHANGE ===============> |
```

---

# 9. What Happens After the Handshake?

Once the connection is established:

```text
Client
   |
   | TCP data
   |-------------------->
   |
   |<--------------------
   | TCP data / ACK
   |
Server
```

TCP tracks data using sequence numbers and acknowledgments.

If data is lost, TCP can retransmit it.

Simplified concept:

```text
Packet 1 → received
Packet 2 → received
Packet 3 → lost
Packet 4 → received

TCP detects the missing data
        ↓
Retransmission
        ↓
Packet 3 sent again
```

---

# 10. Netcat Practical

Start a TCP listener in Terminal 1:

```bash
nc -lvnp 4444
```

Find the Kali IP:

```bash
ip -br a
```

Example:

```text
eth0    UP    10.0.2.15/24
```

Connect from Terminal 2:

```bash
nc 10.0.2.15 4444
```

Now type:

```text
Hello
```

The message should appear in the other terminal.

---

# 11. Observe the TCP Connection

While Netcat is connected:

```bash
ss -tn
```

Look for:

```text
ESTAB
```

This means the TCP connection is established.

The handshake happened before the connection reached this state.

Netcat normally does not display the SYN/SYN-ACK/ACK packets directly. The operating system's TCP stack handles them.

---

# 12. Handshake Concept in Netcat

```text
Terminal 2                         Terminal 1
Client                             Server

   |                                  |
   | ----------- SYN ---------------> |
   | <--------- SYN-ACK ------------- |
   | ----------- ACK ---------------> |
   |                                  |
   | ===== Connection Established ====|
   |                                  |
   | -------- Hello ----------------> |
   |                                  |
```

---

# 13. Viewing the Handshake in Wireshark

Start Wireshark and capture traffic on the active interface.

Then create the Netcat connection.

Use this display filter:

```text
tcp
```

You can identify the handshake by looking for:

```text
[SYN]
[SYN, ACK]
[ACK]
```

The sequence is:

```text
SYN
 ↓
SYN-ACK
 ↓
ACK
 ↓
TCP connection established
```

---

# 14. Useful Commands

Check interfaces:

```bash
ip -br a
```

View listening TCP/UDP sockets:

```bash
ss -tuln
```

View TCP connections:

```bash
ss -tn
```

Start Netcat listener:

```bash
nc -lvnp 4444
```

Connect to listener:

```bash
nc <kali-ip> 4444
```

---

# 15. Security Perspective

Understanding TCP and UDP is important for cybersecurity.

During network analysis, security analysts may examine:

- Source IP
- Destination IP
- Source port
- Destination port
- TCP flags
- Connection state
- Packet frequency
- Unexpected services
- Unusual ports

For example:

```text
Source IP        Destination IP       Destination Port

192.168.1.20  →  192.168.1.50        22
```

This could indicate an SSH connection.

However, the port number alone does not prove which service is actually running on that port.

---

# 16. Interview Answer

### What is the TCP three-way handshake?

The TCP three-way handshake is the process used to establish a TCP connection.

First, the client sends a **SYN** packet.

The server responds with **SYN-ACK**.

Finally, the client sends an **ACK**.

After these three steps, the TCP connection is established and data exchange can begin.

Short version:

```text
SYN → SYN-ACK → ACK → Connection Established
```

---

# 17. Key Takeaways

```text
Transport Layer
      |
      +------ TCP
      |        |
      |        +-- Connection-oriented
      |        +-- Reliable
      |        +-- Ordered
      |        +-- ACKs
      |        +-- Retransmission
      |
      +------ UDP
               |
               +-- Connectionless
               +-- Lower overhead
               +-- No TCP-style reliability
```

TCP handshake:

```text
SYN
 ↓
SYN-ACK
 ↓
ACK
 ↓
Connection Established
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

# 18. Progress

- [x] Learned TCP
- [x] Learned UDP
- [x] Learned ports
- [x] Learned TCP three-way handshake
- [x] Practiced Netcat
- [ ] Capture TCP handshake in Wireshark
- [ ] Complete TryHackMe Tasks 7–9
- [ ] Memorize common ports
- [ ] Add common-ports.md to networking-lab

---

# Day 10 Summary

The main mental model:

```text
IP Address
    ↓
Identifies the host
    ↓
Port
    ↓
Identifies the communication endpoint/service
    ↓
TCP / UDP
    ↓
Transport communication
```

For TCP:

```text
SYN
 ↓
SYN-ACK
 ↓
ACK
 ↓
DATA
```

For UDP:

```text
DATA
 ↓
No TCP-style handshake
```
