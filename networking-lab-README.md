# Networking Lab

**Networking fundamentals notes + packet capture exercises.**

This repository documents my hands-on journey learning computer networking for cybersecurity, SOC analysis, web application security, and network traffic investigation. It combines conceptual notes, Kali Linux exercises, and Wireshark packet analysis.

## Learning Objectives

- Understand the OSI and TCP/IP models and how protocols interact.
- Configure and troubleshoot IP addressing, subnetting, routing, and DNS.
- Compare TCP and UDP and recognize common ports and services.
- Capture network traffic and investigate packets using Wireshark.
- Build a reproducible portfolio of networking labs and observations.

## Topics Covered

| Topic | Concepts and exercises |
| --- | --- |
| OSI Model | Seven layers, encapsulation, protocol examples |
| IP Networking | IPv4, subnet masks, CIDR, private/public IPs |
| Local Networking | MAC addresses, ARP, switches, gateways |
| Network Services | DHCP DORA, DNS records and resolution |
| Transport | TCP handshake, UDP, common ports |
| Web Networking | HTTP, HTTPS, TLS, `curl -v` |
| Virtualization | NAT vs bridged networking in VirtualBox |
| Packet Analysis | Wireshark capture, filters, TCP/DNS/TLS inspection |

## Tools

- **Kali Linux** and **VirtualBox** for the lab environment
- **Wireshark** for capturing and analyzing packets
- **Terminal tools:** `ip`, `ss`, `ping`, `dig`, `nslookup`, `curl`, `nmap`

## Repository Structure

```text
networking-lab/
├── README.md
├── day08-osi-model.md
├── day08-networking-practical.md
├── notes/
│   ├── tcp-udp.md
│   ├── dns-dhcp.md
│   └── wireshark-basics.md
└── captures/
    └── README.md
```

> Planned structure: files and folders are added as each lab is completed. Avoid committing captures containing credentials, private traffic, or other sensitive information.

## Lab 01 — OSI Model

Learn the seven layers and identify example technologies or protocols:

| Layer | Name | Example |
| --- | --- | --- |
| 7 | Application | HTTP |
| 6 | Presentation | TLS (conceptual example) |
| 5 | Session | RPC (conceptual example) |
| 4 | Transport | TCP, UDP |
| 3 | Network | IPv4, IPv6 |
| 2 | Data Link | Ethernet |
| 1 | Physical | Ethernet PHY |

The OSI model is conceptual; real protocols such as TLS and RPC do not always fit neatly into one layer.

**Exercise:** Draw all seven layers and explain what each layer does in one sentence. Record the result in `day08-osi-model.md`.

## Lab 02 — Inspect Kali Network Configuration

```bash
ip -br a             # Show interfaces and IP addresses
ip route              # Show routes and default gateway
ip neigh              # Inspect local neighbor mappings
ss -tulnp             # Inspect listening TCP/UDP sockets
ping -c 4 8.8.8.8     # Test IP connectivity
nslookup example.com  # Resolve a domain
```

**Record:** Active interface, private IP, prefix, default gateway, and the difference between NAT and bridged networking. Never publish private infrastructure details that should remain confidential.

## Lab 03 — First Wireshark Capture

1. Confirm Wireshark is installed with `which wireshark`.
2. Find the active interface using `ip -br a`.
3. Start a Wireshark capture on that interface.
4. Browse `https://example.com` inside Kali for approximately 60 seconds.
5. Generate test traffic in a terminal:

   ```bash
   dig example.com
   ping -c 4 8.8.8.8
   curl -v https://example.com
   ```

6. Stop the capture and inspect the packets.

### Wireshark Display Filters

| Filter | Purpose |
| --- | --- |
| `dns` | DNS queries and responses |
| `tcp` | TCP packets |
| `udp` | UDP packets |
| `icmp` | IPv4 ICMP traffic |
| `http` | Recognized HTTP traffic |
| `tls` | Recognized TLS traffic |
| `tcp.port == 80` | TCP traffic using port 80 |
| `tcp.port == 443` | TCP traffic using port 443 |
| `ip.addr == 8.8.8.8` | IPv4 traffic to or from the specified address |
| `tcp.flags.syn == 1 && tcp.flags.ack == 0` | Initial TCP SYN packets |

**Capture filters** determine which packets are recorded; **display filters** determine which recorded packets are shown. They use different syntax.

### Questions to Answer

- Which IP addresses communicated with the VM?
- Can I identify a DNS query and its response?
- Can I find a TCP SYN → SYN-ACK → ACK sequence?
- What does the TLS Client Hello reveal?
- Why isn't normal HTTPS application content readable without decryption?
- What changes when I apply `http` versus `tcp.port == 80`?

> Note: Browser caching, encrypted DNS, HTTP/3 over QUIC, and connection reuse can change which packets appear. If a particular protocol is missing, generate explicit test traffic rather than assuming the capture failed.

## Security Investigation Mindset

When analyzing a capture, consider the source, destination, protocol, port, timing, volume, and whether the behavior is expected. Potential investigation leads include repeated connection attempts, unusual DNS patterns, unexpected destinations, and unexplained outbound data transfers. A single unusual packet or port is not proof of malicious activity.

Capture only traffic on systems and networks you own or are authorized to analyze.

## Learning Resources

- [Cisco Skills for All](https://skillsforall.com/) — Networking Basics
- [Practical Networking](https://www.practicalnetworking.net/) — networking explanations and OSI material
- [TryHackMe: Network Fundamentals](https://tryhackme.com/module/network-fundamentals) — guided exercises
- [Wireshark](https://www.wireshark.org/) — packet analyzer and documentation

## Progress Tracker

- [ ] Document OSI layers and protocol examples
- [ ] Inspect Kali network interfaces and routing
- [ ] Complete a 60-second Wireshark capture
- [ ] Practice DNS, TCP, ICMP, HTTP, and TLS display filters
- [ ] Identify a TCP three-way handshake
- [ ] Write observations from the first packet analysis lab

## Goal

Create a practical networking portfolio that demonstrates the ability to explain protocols, troubleshoot connectivity, capture packets, and investigate network behavior using real lab evidence.
