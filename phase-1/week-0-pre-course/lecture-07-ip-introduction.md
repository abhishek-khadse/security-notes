# Lecture 07: Introduction to IP
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## What is IP?
Internet Protocol defines how devices are identified and how packets are routed across networks.
IP works at OSI Layer 3, the Network layer.

## IPv4 vs IPv6
| Feature | IPv4 | IPv6 |
|---------|------|------|
| Size | 32-bit | 128-bit |
| Format | Decimal | Hexadecimal |
| Example | 192.168.1.1 | 2001:db8::1 |
| Address Space | About 4.3 billion | Extremely large |
| Security | Supports IPsec | Supports IPsec, but encryption is not automatic |

## RFC1918 Private IP Ranges
| Range | CIDR | Common Use |
|-------|------|------------|
| 10.0.0.0 | /8 | Large private networks |
| 172.16.0.0 | /12 | Medium private networks |
| 192.168.0.0 | /16 | Home and small office networks |

## Key Networking Terms
| Term | Meaning |
|------|---------|
| IP Address | Logical Layer 3 device identifier |
| MAC Address | Physical Layer 2 hardware address |
| Subnet Mask | Separates network and host portions |
| Default Gateway | Router used to reach other networks |
| DNS | Converts names to IP addresses |
| DHCP | Automatically assigns network settings |

## Common Subnet Masks
| Subnet Mask | CIDR | Usable Hosts |
|-------------|------|--------------|
| 255.0.0.0 | /8 | 16,777,214 |
| 255.255.0.0 | /16 | 65,534 |
| 255.255.255.0 | /24 | 254 |

## Offensive Angle
| Concept | Example Attack |
|---------|----------------|
| IP Address | IP spoofing, geolocation bypass |
| DHCP | Rogue DHCP server attack |
| DNS | DNS poisoning, DNS tunneling |
| ARP | ARP spoofing -> MITM |
| ICMP | Ping sweep, ping flood |

### Recon Commands
```bash
# Network discovery
nmap -sn 192.168.1.0/24

# DNS lookup
nslookup target.com
dig target.com

# ARP table
arp -a

# Trace route
traceroute target.com
```

## SOC Detection
| Attack | Alert Idea |
|--------|------------|
| IP Spoofing | Impossible or invalid source IP |
| DHCP Attack | Rogue DHCP server detected |
| DNS Poisoning | DNS response mismatch |
| ICMP Flood | High ICMP volume |
| ARP Spoofing | Gratuitous ARP or MAC/IP mismatch |

## Interview Questions
**Q: What is the difference between MAC and IP address?**
MAC is a Layer 2 physical address used on the local network.
IP is a Layer 3 logical address used for routing between networks.

**Q: What is DHCP?**
DHCP automatically assigns an IP address, subnet mask, default gateway, and DNS servers to devices.

**Q: What is a default gateway?**
The default gateway is the router IP address used to forward traffic outside the local network.
