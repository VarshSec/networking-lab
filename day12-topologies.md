# Day 12 — Network Devices, Topologies & NAT vs Bridged

## Objective

Understand:

- Router
- Switch
- Firewall
- Star topology
- Mesh topology
- Bus topology
- NAT vs Bridged networking in VirtualBox
- Practical network troubleshooting

---

# 1. Network Devices

## Router

A router connects different IP networks and forwards packets between them.

Example:

```text
LAN
 |
Router
 |
Internet
```

A router commonly acts as the default gateway for devices on a local network.

---

## Switch

A switch connects devices within a local network.

It primarily uses MAC addresses to forward Ethernet frames.

Example:

```text
        Switch
       /  |  \
     PC  PC  Printer
```

---

## Firewall

A firewall controls network traffic according to configured security rules.

It can allow or block traffic based on factors such as:

- Source IP
- Destination IP
- Port
- Protocol
- Connection state
- Security policy

Example:

```text
Internet
    |
 Firewall
    |
   LAN
```

A firewall and router are different concepts, although modern network devices can combine both functions.

---

# 2. Network Topologies

A network topology describes how devices are connected.

Common topologies include:

- Star
- Mesh
- Bus

---

# 3. Star Topology

In a star topology, devices connect to a central device, usually a switch.

```text
             PC
              |
              |
PC -------- Switch -------- PC
              |
              |
           Printer
```

### Advantages

- Easy to manage
- Easy to add devices
- Failure of one endpoint link usually affects only that device

### Disadvantage

If the central device fails, connected devices can lose network connectivity.

---

# 4. Mesh Topology

In a mesh topology, devices have multiple connections to other devices.

```text
      A -------- B
       \        / \
        \      /   \
         \    /     \
          C -------- D
```

The purpose of multiple paths is redundancy.

If one link fails, another path may still exist.

A full mesh with `n` devices requires:

```text
n(n - 1) / 2
```

links.

---

# 5. Bus Topology

In a bus topology, devices share a common communication backbone.

```text
PC ---- PC ---- PC ---- PC
       Shared Bus
```

Bus topology was common in older Ethernet networks.

A failure or problem with the shared backbone can affect multiple devices.

---

# 6. Topology Comparison

| Topology | Structure | Main Characteristic |
|---|---|---|
| Star | Central device | Easy management |
| Mesh | Multiple interconnections | Redundancy |
| Bus | Shared backbone | Shared medium |

Mental model:

```text
Star → Central point
Mesh → Multiple paths
Bus → Shared backbone
```

---

# 7. VirtualBox Practical — NAT vs Bridged

The VirtualBox network adapter can operate in different modes.

Two important modes are:

- NAT
- Bridged Adapter

---

# 8. NAT Mode

In NAT mode, the VM is placed behind VirtualBox's virtual NAT network.

Example:

```text
Kali VM
10.0.2.15
    |
VirtualBox NAT
    |
Host Machine
    |
Home Network
    |
Internet
```

The VM can normally access external networks through the host's connection.

The VM is not directly presented as an independent device on the physical home LAN.

---

# 9. Bridged Mode

In Bridged mode, the VM connects to the physical network through the host's network adapter.

Example:

```text
             Home Router
                  |
        ---------------------
        |         |         |
      Laptop    Phone     Kali VM
                           |
                       Separate
                       LAN IP
```

The VM can receive an IP address from the same LAN's DHCP infrastructure, depending on the network.

---

# 10. Practical Task

Before changing the adapter, check the current address:

```bash
ip -br a
```

Or:

```bash
ip a
```

Record the IP address.

Example NAT address:

```text
eth0    UP    10.0.2.15/24
```

---

## Change to Bridged

In VirtualBox:

```text
VM Settings
    ↓
Network
    ↓
Adapter 1
    ↓
Attached to: Bridged Adapter
    ↓
Select your active Wi-Fi/Ethernet adapter
```

Start Kali and run:

```bash
ip -br a
```

Then:

```bash
ip route
```

You may see an address from your home network, for example:

```text
10.34.126.100/24
```

The exact address depends on your network.

Check the default gateway:

```bash
ip route
```

Example:

```text
default via 10.34.126.136 dev eth0
```

Test the gateway:

```bash
ping -c 4 10.34.126.136
```

Test Internet connectivity:

```bash
ping -c 4 8.8.8.8
```

Test DNS:

```bash
ping -c 4 google.com
```

---

# 11. Switch Back to NAT

After completing the bridged experiment:

```text
VirtualBox
    ↓
VM Settings
    ↓
Network
    ↓
Adapter 1
    ↓
Attached to: NAT
```

Restart the network interface or reboot Kali if necessary.

Then verify:

```bash
ip -br a
```

You may again see a VirtualBox NAT address such as:

```text
10.0.2.15/24
```

---

# 12. NAT vs Bridged Comparison

| Feature | NAT | Bridged |
|---|---|---|
| VM network position | Behind VirtualBox NAT | Directly connected to LAN |
| Typical VM IP | VirtualBox private network | Home/LAN network address |
| Appears as separate LAN device | Usually no | Yes |
| Internet access | Yes | Yes, if LAN permits |
| Useful for | Normal VM usage | LAN/networking labs |

---

# 13. My Observation

During the VirtualBox practical, the Kali VM initially used NAT networking.

The VM received an address such as:

```text
10.0.2.15/24
```

After switching to Bridged mode, the VM received an address from the physical/home network, such as:

```text
10.34.126.100/24
```

The default gateway also changed to a gateway on the bridged network, for example:

```text
default via 10.34.126.136
```

After testing connectivity, switching back to NAT returned the VM to the VirtualBox NAT network.

---

# 14. Why This Matters for Cybersecurity

Understanding network topology and virtualization networking is useful for:

- Network reconnaissance
- Packet analysis
- Firewall testing
- Network troubleshooting
- Virtual lab design
- Understanding attack surfaces
- Understanding routing and segmentation

For authorized labs, Bridged mode can make a VM behave like another device on the LAN.

NAT can provide a more isolated virtual network behind the host.

---

# 15. Useful Commands

Show interfaces:

```bash
ip -br a
```

Show full interface information:

```bash
ip a
```

Show routing table:

```bash
ip route
```

Show neighbors:

```bash
ip neigh
```

Test gateway:

```bash
ping -c 4 <gateway-ip>
```

Test Internet connectivity:

```bash
ping -c 4 8.8.8.8
```

Test DNS:

```bash
ping -c 4 google.com
```

---

# 16. Interview Answers

### What is a router?

A router connects different IP networks and forwards packets between them.

### What is a switch?

A switch connects devices within a LAN and primarily forwards Ethernet frames using MAC addresses.

### What is a firewall?

A firewall controls network traffic according to security rules and can allow or block connections.

### Star vs Mesh?

Star topology uses a central device, while mesh topology provides multiple paths between devices for redundancy.

### NAT vs Bridged?

NAT places the VM behind a virtual NAT network, while Bridged mode connects the VM directly to the physical LAN so it can appear as a separate device on that network.

---

# 17. Progress

- [x] Learned routers
- [x] Learned switches
- [x] Learned firewalls
- [x] Learned star topology
- [x] Learned mesh topology
- [x] Learned bus topology
- [x] Practiced VirtualBox NAT
- [x] Practiced VirtualBox Bridged
- [x] Compared VM IP addresses
- [x] Checked routing table
- [x] Tested gateway connectivity
- [ ] Complete TryHackMe Network Fundamentals final assessment
- [ ] Update networking-lab README

---

# Day 12 Summary

```text
Network Devices

Router
  ↓
Connects different IP networks

Switch
  ↓
Connects devices within a LAN

Firewall
  ↓
Controls network traffic

Topologies

Star
  ↓
Central device

Mesh
  ↓
Multiple paths

Bus
  ↓
Shared backbone
```

VirtualBox:

```text
NAT
 ↓
VM → VirtualBox NAT → Host → Network

Bridged
 ↓
VM → Physical LAN
```

The key practical difference is that Bridged mode makes the VM participate directly in the physical LAN, while NAT places it behind VirtualBox's virtual NAT network.
