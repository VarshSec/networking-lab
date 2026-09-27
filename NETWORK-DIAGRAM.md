# Network Diagram — Home Lab

## Purpose

Document the basic home-lab topology used during the networking fundamentals exercises.

## Topology

```text
                    Internet
                       |
                       |
                Home Network / Router
                       |
                       |
                  Host PC / Laptop
                       |
                  VirtualBox
                       |
                 NAT Network
                       |
                       |
                   Kali VM
                       |
              eth0 / Virtual NIC
```

## Main Components

### Host PC

The physical computer running VirtualBox.

### VirtualBox

The virtualization platform hosting the Kali Linux VM.

### NAT Network

VirtualBox provides network address translation between the Kali VM and the host's external network connection.

### Kali VM

The Linux virtual machine used for networking, cybersecurity, and packet-analysis labs.

## NAT Behavior

In the normal NAT configuration:

```text
Kali VM
   ↓
VirtualBox NAT
   ↓
Host PC
   ↓
Home Network
   ↓
Internet
```

The VM uses a VirtualBox-managed private network.

Example previously observed NAT address:

```text
10.0.2.15/24
```

## Bridged Experiment

During the Day 12 practical, the adapter was temporarily changed to Bridged mode:

```text
Kali VM
   ↓
VirtualBox Bridged Adapter
   ↓
Physical Wi-Fi/Ethernet
   ↓
Home Network
```

In Bridged mode, Kali can receive an address from the physical LAN's network infrastructure.

After the experiment, the VM was switched back to NAT.

## Security / Learning Purpose

This topology is used for:

- Networking fundamentals
- Subnetting practice
- TCP/UDP exercises
- DNS and HTTP testing
- Wireshark packet analysis
- Virtual network experimentation

All testing should remain within systems and networks the user is authorized to access.
