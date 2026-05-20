# Lecture 25: IPv6 Addressing
Date: May 20, 2026
Source: NetworkChuck Free CCNA

## IPv4 vs IPv6
| Feature | IPv4 | IPv6 |
|---------|------|------|
| Bits | 32-bit | 128-bit |
| Format | Decimal | Hexadecimal |
| Addresses | 4.3 billion | 340 undecillion |
| NAT needed | Yes | No |
| ARP | ARP | NDP |
| Auto-config | DHCP | SLAAC |
| Security | Optional IPSec | Built-in IPSec |

## IPv6 Address Format
```
2001:0db8:85a3:0000:0000:8a2e:0370:7334
↑    ↑    ↑    ↑    ↑    ↑    ↑    ↑
8 groups × 16 bits = 128 bits total
```

## Shortening Rules
Rule 1: Remove leading zeros
2001:0db8:0001 → 2001:db8:1
Rule 2: Replace consecutive zero groups with ::
2001:0000:0000:0000:0000:0000:0000:0001
→ 2001::1
IMPORTANT: :: can only be used ONCE per address

## IPv6 Address Types
| Type | Prefix | Purpose |
|------|--------|---------|
| Global Unicast | 2000::/3 | Public internet (like IPv4 public) |
| Link-Local | FE80::/10 | Local network only, auto-assigned |
| Unique Local | FC00::/7 | Private network (like IPv4 private) |
| Loopback | ::1 | Local machine (like 127.0.0.1) |
| Unspecified | :: | Like 0.0.0.0 |
| Multicast | FF00::/8 | One to many |
| Anycast | Any unicast | Nearest destination |

## Common Prefixes
| Prefix | Use |
|--------|-----|
| /64 | Standard subnet |
| /128 | Single host |
| /48 | Large organization |

## IPv6 Key Protocols
| Protocol | IPv4 Equivalent | Purpose |
|----------|-----------------|---------|
| NDP | ARP | Neighbor/router discovery |
| SLAAC | DHCP | Auto address config |
| DHCPv6 | DHCP | Managed address config |
| ICMPv6 | ICMP | Error reporting + NDP |

## Transition Methods
| Method | How | Use |
|--------|-----|-----|
| Dual Stack | Run both IPv4 + IPv6 | Most common |
| Tunneling | IPv6 inside IPv4 packets | Gradual migration |
| Translation | NAT64 converts between them | Last resort |

## Commands
```bash
# Windows
ipconfig              # Show IPv6 addresses
ping -6 ::1           # Ping IPv6 loopback
tracert -6 target     # IPv6 traceroute

# Linux
ip -6 addr            # Show IPv6 interfaces
ping6 ::1             # Ping loopback
traceroute6 target    # IPv6 traceroute
```

## Offensive Angle
| Attack | Description |
|--------|-------------|
| NDP spoofing | Like ARP spoofing but for IPv6 |
| SLAAC attack | Rogue router sends fake router advertisement |
| IPv6 tunnel abuse | Hide IPv6 traffic inside IPv4 |
| Link-local recon | FE80:: addresses expose all local devices |
| Dual stack bypass | Target IPv6 when IPv4 is protected |
| ICMPv6 flood | DoS via ICMPv6 packets |

### NDP Spoofing — ARP Spoofing for IPv6
IPv4: ARP spoofing redirects traffic
IPv6: NDP spoofing does the same thing
Attacker sends fake Neighbor Advertisement
Claims to own target's IPv6 address
All traffic redirected to attacker = MITM
Tool: fake_router6 (THC-IPv6 toolkit)
fake_router6 eth0 2001:db8::/64

### Dual Stack Bypass Attack
Company protects IPv4 with firewall rules
Forgot to configure IPv6 firewall rules
Both IPv4 + IPv6 active on servers
Attacker connects via IPv6
Bypasses all IPv4 firewall rules
Gets direct access to server
Common mistake in enterprise networks
Check: ip -6 addr on every server

### SLAAC Rogue Router Attack
IPv6 devices use Router Advertisements
to auto-configure addresses (SLAAC)
Attacker sends fake RA:
"I am your router, use me as gateway"
Devices auto-configure with attacker as gateway
All traffic flows through attacker = MITM
Tool: radvd or fake_router6

### IPv6 Recon
```bash
# Find all IPv6 hosts on local network
nmap -6 -sn fe80::/10

# Scan IPv6 target
nmap -6 2001:db8::1

# Find link-local addresses
ping6 -I eth0 ff02::1    # All nodes multicast
```

## SOC Detection
| Attack | Detection |
|--------|-----------|
| NDP spoofing | Duplicate IPv6 address alert |
| Rogue RA | Unexpected router advertisement |
| IPv6 tunnel | IPv6-in-IPv4 traffic on network |
| Dual stack bypass | IPv6 connections to IPv4-only servers |
| ICMPv6 flood | High ICMPv6 volume alert |
| Link-local scan | ff02::1 ping = recon |

### Key SOC Rules for IPv6
Rule 1: Multiple devices claiming same IPv6 = NDP spoof
Rule 2: Unknown router advertisement source = rogue RA
Rule 3: IPv6 traffic on IPv4-only segment = tunnel
Rule 4: ff02::1 ping = network recon
Rule 5: ICMPv6 flood > 100/sec = DoS alert
Rule 6: FE80 traffic crossing subnets = anomaly

## Interview Questions
**Q: What replaces ARP in IPv6?**
NDP — Neighbor Discovery Protocol.
Uses ICMPv6 messages.
Vulnerable to NDP spoofing like ARP spoofing.

**Q: What is SLAAC?**
Stateless Address Autoconfiguration.
IPv6 device generates own address using:
- Router prefix from RA message
- Device MAC address (EUI-64)
No DHCP server needed.

**Q: What is the IPv6 loopback?**
::1 (equivalent of 127.0.0.1 in IPv4)

**Q: Why is dual stack a security risk?**
Organizations secure IPv4 carefully.
Often forget IPv6 firewall rules.
Attackers target IPv6 to bypass IPv4 controls.

**Q: How many IPv6 addresses exist?**
2^128 = 340 undecillion addresses.
Enough for every grain of sand on Earth
to have billions of addresses.
