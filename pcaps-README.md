# Packet Capture Exercises

## Day 13 — Wireshark Intro

### Capture

- Tool: Wireshark
- Interface: `eth0` (replace with the interface actually used)
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
