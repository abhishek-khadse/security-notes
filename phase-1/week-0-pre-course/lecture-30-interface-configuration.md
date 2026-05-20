# Lecture 30: Interface Configuration
Date: May 20, 2026
Source: NetworkChuck Free CCNA

## Interface Types
| Interface | Purpose | Example |
|-----------|---------|---------|
| Ethernet | LAN communication | g0/0, fa0/1 |
| Serial | WAN links | s0/0/0 |
| Loopback | Virtual/testing | lo0 |
| VLAN Interface | L3 VLAN routing | vlan10 |
| Subinterface | Router-on-a-stick | g0/0.10 |

## Basic Config Workflow
```bash
# 1. Enter interface
interface g0/0

# 2. Add description
description Connection_to_Core_Switch

# 3. Assign IP
ip address 192.168.1.1 255.255.255.0

# 4. Enable it
no shutdown

# 5. Verify
show ip interface brief
```

## Interface Status
| Status | Meaning |
|--------|---------|
| up/up | Working perfectly |
| up/down | Layer 2 problem |
| down/down | Physical problem |
| admin down | Manually shut down |

## Common Configurations
```bash
# Full duplex + speed
duplex full
speed 1000

# Loopback (always up, used for management)
interface loopback0
ip address 1.1.1.1 255.255.255.255
no shutdown

# Subinterface (Router-on-a-stick)
interface g0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0

# Serial WAN link
interface s0/0/0
ip address 10.0.0.1 255.255.255.252
clock rate 64000
no shutdown

# IPv6
interface g0/0
ipv6 address 2001:db8::1/64
no shutdown

# DHCP client
interface g0/0
ip address dhcp

# Layer 3 VLAN interface
interface vlan 10
ip address 192.168.10.1 255.255.255.0
no shutdown
```

## Port Security
```bash
# Basic port security
interface fa0/1
switchport mode access
switchport port-security
switchport port-security maximum 1
switchport port-security violation shutdown
switchport port-security mac-address sticky

# Shut unused ports
interface range fa0/10-24
shutdown
description UNUSED_PORT
switchport access vlan 999
```

## Troubleshooting Commands
```bash
# Cisco
show ip interface brief      # Quick status all interfaces
show interfaces g0/0         # Detailed single interface
show running-config          # Full config
ping 192.168.1.2             # Test connectivity
traceroute 8.8.8.8           # Trace path

# Linux
ip a                         # Show interfaces
ip link set eth0 up          # Enable interface
ethtool eth0                 # Interface details
ping -I eth0 192.168.1.1     # Ping via specific interface

# Windows
ipconfig /all                # Show all interfaces
netsh interface show interface
ping 192.168.1.1
```

## Offensive Angle
| Attack | Description |
|--------|-------------|
| Promiscuous mode | Capture all traffic on interface |
| MAC spoofing | Change interface MAC address |
| IP spoofing | Change source IP on interface |
| Port security bypass | Spoof allowed MAC address |
| Loopback abuse | Use loopback for covert communication |
| MTU mismatch | Fragment packets to bypass IDS |
| DHCP starvation | Exhaust DHCP pool via interface flooding |

### Promiscuous Mode Attack
```bash
# Normal interface: only accepts own traffic
# Promiscuous mode: captures ALL traffic

# Enable on Kali
ip link set eth0 promisc on

# Now capture everything with Wireshark
# Or tcpdump
tcpdump -i eth0 -w capture.pcap

# This is how:
# - Wireshark works
# - Network sniffing works
# - MITM traffic capture works
```

### MAC Spoofing on Interface
```bash
# Change MAC to bypass port security
ip link set eth0 down
ip link set eth0 address 00:11:22:33:44:55
ip link set eth0 up

# Now you appear as allowed device
# Bypass MAC-based access control
# Bypass port security (if you know allowed MAC)

# Find allowed MAC first via:
nmap --script broadcast-dhcp-discover
arp-scan -l
```

### MTU Manipulation to Bypass IDS
```bash
# IDS looks for attack signatures in packets
# If you fragment below IDS inspection threshold
# IDS misses the attack

# Nmap fragmentation
nmap -f target.com          # Fragment packets
nmap --mtu 8 target.com     # Set tiny MTU

# Some IDS can't reassemble fragments
# Attack slips through undetected
```

### DHCP Starvation
```bash
# Tool: Yersinia or DHCPig
# Send thousands of DHCP requests
# Each with different spoofed MAC
# DHCP pool exhausted
# Legitimate users can't get IP = DoS

# Then: set up rogue DHCP server
# Hand out attacker as default gateway
# Full MITM position
```

## SOC Detection
| Attack | Detection |
|--------|-----------|
| Promiscuous mode | NIC in promisc mode alert |
| MAC spoofing | MAC change on port detected |
| Port security violation | Shutdown alert on port |
| DHCP starvation | DHCP pool exhaustion alert |
| MTU manipulation | Fragmented packet spike |
| IP spoofing | Source IP validation failure |

### Key SOC Rules
Rule 1: Interface enters promiscuous mode = alert
Rule 2: MAC change on switch port = investigate
Rule 3: Port security violation = lock + alert
Rule 4: DHCP requests > 100/min from 1 port = starvation
Rule 5: Fragmented packets spike = evasion attempt
Rule 6: Interface flapping = physical issue or attack

## Common Problems + Fixes
| Problem | Cause | Fix |
|---------|-------|-----|
| Interface down | Shutdown state | no shutdown |
| No connectivity | Wrong IP/mask | Verify ip address |
| Duplex mismatch | Speed/duplex mismatch | Set manually both sides |
| VLAN mismatch | Wrong VLAN assigned | Check switchport access vlan |
| Port security violation | Wrong MAC connected | Clear violation or reconfigure |

## Interview Questions
**Q: What does no shutdown do?**
Enables interface that is administratively down.
Default state on Cisco = shutdown.
Must explicitly enable every interface.

**Q: What is a loopback interface?**
Virtual interface that's always up.
Never goes down unless manually shut.
Used for: router ID, management access,
testing, BGP peering address.

**Q: What is port security?**
Restricts which MAC addresses can use a port.
Maximum 1 device per port typically.
Violation options: shutdown, restrict, protect.
Sticky MAC learns and remembers allowed MACs.

**Q: What is promiscuous mode?**
Interface accepts ALL traffic regardless of
destination MAC, not just traffic destined for it.
Used by: Wireshark, IDS sensors, network taps.
Security risk: enables passive sniffing.
