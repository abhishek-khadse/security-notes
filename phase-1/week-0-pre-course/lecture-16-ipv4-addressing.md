# Lecture 16: IPv4 Addressing
Date: May 20, 2026
Source: NetworkChuck Free CCNA

## IPv4 Basics
- 32-bit address, 4 octets, decimal format
- Example: 192.168.1.10
- Binary: 11000000.10101000.00000001.00001010

## Address Classes
| Class | Range | Mask | CIDR | Use |
|-------|-------|------|------|-----|
| A | 1-126.x.x.x | 255.0.0.0 | /8 | Large networks |
| B | 128-191.x.x.x | 255.255.0.0 | /16 | Medium networks |
| C | 192-223.x.x.x | 255.255.255.0 | /24 | Small networks |
| D | 224-239.x.x.x | - | - | Multicast |
| E | 240-255.x.x.x | - | - | Research |

## Special Addresses
| Address | Purpose |
|---------|---------|
| 127.0.0.1 | Loopback — test local machine |
| 169.254.x.x | APIPA — DHCP failed |
| 192.168.1.0 | Network address |
| 192.168.1.255 | Broadcast address |
| 192.168.1.1 | Default gateway (typically) |

## Private Ranges
| Class | Range | Use |
|-------|-------|-----|
| A | 10.0.0.0/8 | Large enterprise |
| B | 172.16.0.0/12 | Medium networks |
| C | 192.168.0.0/16 | Home/office |

## IPv4 Packet Structure
| Field | Purpose |
|-------|---------|
| Source IP | Who sent it |
| Destination IP | Where it's going |
| TTL | Prevents infinite loops |
| Protocol | TCP/UDP/ICMP |
| Header Checksum | Error detection |

## Key Protocols
| Protocol | Purpose | Port |
|----------|---------|------|
| ICMP | Diagnostics, ping | - |
| ARP | IP → MAC mapping | - |
| DHCP | Auto IP assignment | 67/68 |
| DNS | Domain → IP | 53 |
| NAT | Private ↔ Public IP | - |

## Commands
```bash
# Windows
ipconfig          # Show IP config
arp -a            # Show ARP table
tracert           # Trace route
nslookup          # DNS lookup

# Linux
ip a              # Show interfaces
arp               # ARP table
traceroute        # Trace route
dig               # Detailed DNS lookup
```

## Offensive Angle
| Concept | Attack |
|---------|--------|
| Public IP | Direct attack surface |
| ICMP | Ping sweep, OS fingerprinting |
| ARP | ARP spoofing → MITM |
| DHCP | Rogue DHCP → redirect traffic |
| TTL | OS fingerprinting via TTL values |
| APIPA | Detect DHCP failure → attack window |
| NAT bypass | Source routing, tunneling |

### TTL OS Fingerprinting
TTL = 64  → Linux/Unix
TTL = 128 → Windows
TTL = 255 → Cisco/Network device
nmap -O target.com  # Detect OS via TTL

### Rogue DHCP Attack
Attacker sets up fake DHCP server
Victim requests IP address
Rogue DHCP responds first
Assigns:

Attacker as default gateway
Attacker as DNS server
= All traffic flows through attacker
= Full MITM position

### ICMP Recon
```bash
# Ping sweep — find live hosts
nmap -sn 192.168.1.0/24

# OS detection via TTL
ping 192.168.1.1

# Traceroute — map network
traceroute target.com
```

## SOC Detection
| Attack | Detection |
|--------|-----------|
| ICMP sweep | Mass ping requests alert |
| ARP spoofing | Gratuitous ARP detected |
| Rogue DHCP | Unknown DHCP server on network |
| IP spoofing | Impossible source IP alert |
| TTL scan | Unusual TTL values in packets |

### QRadar/Sentinel Rules
Rule 1: Multiple IPs from 1 MAC = spoofing
Rule 2: Unknown DHCP offer = rogue server
Rule 3: ICMP flood > 100/sec = sweep alert
Rule 4: TTL=1 packets in bulk = traceroute recon
Rule 5: Same IP, different MAC = ARP spoof

## Interview Questions
**Q: What is APIPA?**
169.254.x.x range. Automatically assigned
when DHCP server unreachable.
Means DHCP is broken — investigate.

**Q: What is NAT?**
Network Address Translation.
Converts private IPs to public IP for internet.
Conserves IPv4 addresses.
Hides internal network structure.

**Q: What is TTL?**
Time To Live. Decrements by 1 at each hop.
Prevents packets looping forever.
Also used for OS fingerprinting.

**Q: Public vs Private IP?**
Public = internet routable, assigned by ISP,
globally unique, direct attack surface.
Private = internal only, not routable,
reused across millions of networks.

**Q: What does ARP do?**
Maps IP address to MAC address.
Operates at Layer 2/3 boundary.
Vulnerable to spoofing — no authentication.
