# Common Network Ports

## Purpose

Quick reference for commonly encountered network ports and their associated services.

## Common Ports

| Port | Service | Protocol | Purpose |
|---:|---|---|---|
| 21 | FTP | TCP | File Transfer Protocol |
| 22 | SSH | TCP | Secure remote access |
| 23 | Telnet | TCP | Remote terminal access |
| 25 | SMTP | TCP | Sending email |
| 53 | DNS | UDP/TCP | Domain name resolution |
| 80 | HTTP | TCP | Web traffic |
| 443 | HTTPS | TCP | Secure web traffic |
| 3389 | RDP | TCP/UDP | Remote Desktop |

## Quick Memorization

21 → FTP  
22 → SSH  
23 → Telnet  
25 → SMTP  
53 → DNS  
80 → HTTP  
443 → HTTPS  
3389 → RDP

## Categories

### Remote Access

- 22 — SSH
- 23 — Telnet
- 3389 — RDP

### Web

- 80 — HTTP
- 443 — HTTPS

### Network Services

- 53 — DNS

### Email

- 25 — SMTP

### File Transfer

- 21 — FTP

## Security Notes

Port numbers are conventions, not proof of the service running on that port.

For example:

```text
443 → commonly HTTPS
```

but a service can technically listen on another port.

To discover listening ports on a Linux system:

```bash
ss -tuln
```

To inspect a host in an authorized lab:

```bash
nmap <target-ip>
```

## Learning Progress

- [x] TCP vs UDP
- [x] Common ports
- [x] TCP three-way handshake
- [ ] Practice identifying ports in Wireshark
- [ ] Practice identifying ports with `ss`
- [ ] Practice authorized port scanning
