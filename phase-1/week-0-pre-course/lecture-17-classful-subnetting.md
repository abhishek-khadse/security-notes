# Lecture 17: Classful Subnetting
Date: May 20, 2026
Source: NetworkChuck Free CCNA

## IP Address Classes
| Class | Range | Default Mask | CIDR | Networks | Hosts |
|-------|-------|--------------|------|----------|-------|
| A | 1.0.0.0 – 126.255.255.255 | 255.0.0.0 | /8 | 126 | 16M+ |
| B | 128.0.0.0 – 191.255.255.255 | 255.255.0.0 | /16 | 16K | 65K |
| C | 192.0.0.0 – 223.255.255.255 | 255.255.255.0 | /24 | 2M | 254 |
| D | 224.0.0.0 – 239.255.255.255 | - | - | Multicast | - |
| E | 240.0.0.0 – 255.255.255.255 | - | - | Research | - |

## Private IP Ranges
| Class | Private Range | Common Use |
|-------|---------------|------------|
| A | 10.0.0.0/8 | Large enterprise |
| B | 172.16.0.0/12 | Medium networks |
| C | 192.168.0.0/16 | Home/small office |

## Subnet Mask Quick Reference
| CIDR | Subnet Mask | Block Size | Usable Hosts |
|------|-------------|------------|--------------|
| /8 | 255.0.0.0 | 16M | 16M-2 |
| /16 | 255.255.0.0 | 65536 | 65534 |
| /24 | 255.255.255.0 | 256 | 254 |
| /25 | 255.255.255.128 | 128 | 126 |
| /26 | 255.255.255.192 | 64 | 62 |
| /27 | 255.255.255.224 | 32 | 30 |
| /28 | 255.255.255.240 | 16 | 14 |
| /29 | 255.255.255.248 | 8 | 6 |
| /30 | 255.255.255.252 | 4 | 2 |

## Subnetting Formulas
Subnets  = 2^n      (n = borrowed bits)
Hosts    = 2^h - 2  (h = remaining host bits)
Block    = 256 - last octet of mask

## Subnetting Example — 192.168.1.0/24 into 4 subnets
Need 4 subnets → borrow 2 bits → /26
Block size = 256 - 192 = 64
Subnet 1: 192.168.1.0   → 192.168.1.63
Subnet 2: 192.168.1.64  → 192.168.1.127
Subnet 3: 192.168.1.128 → 192.168.1.191
Subnet 4: 192.168.1.192 → 192.168.1.255
Usable hosts per subnet = 2^6 - 2 = 62

## Key Terms
| Term | Meaning |
|------|---------|
| Network ID | First address — identifies subnet |
| Broadcast | Last address — sends to all hosts |
| Valid Hosts | Everything between network + broadcast |
| CIDR | Classless notation e.g. /24 |

## Offensive Angle
| Concept | Attack Use |
|---------|------------|
| Network discovery | Identify all subnets in target org |
| Broadcast address | Smurf attack — amplified DoS |
| Private ranges | Internal recon after initial access |
| Subnet mapping | Plan lateral movement paths |

### Recon Commands
```bash
# Discover all hosts in subnet
nmap -sn 192.168.1.0/24

# Identify network ranges
whois target.com
nmap --script ip-geolocation target.com

# After internal access — map subnets
ip route show
arp -a
netstat -rn
```

### Smurf Attack (Broadcast Abuse)
Attacker spoofs victim IP as source
Sends ICMP to broadcast address
ALL hosts reply to victim
= Amplified DoS attack
Broadcast control prevents this

## SOC Detection
| Attack | Detection |
|--------|-----------|
| Subnet scanning | Sequential IP hits in logs |
| Smurf attack | ICMP flood from multiple sources |
| Internal recon | Unusual ARP requests across subnets |
| Lateral movement | Traffic between isolated subnets |

### QRadar Rules for Subnet Attacks
Rule 1: >50 ICMP requests in 10 seconds = scan
Rule 2: Traffic crossing VLAN boundary = alert
Rule 3: New internal IP communicating = flag
Rule 4: Broadcast flood = DoS alert

## Interview Questions
**Q: Why subtract 2 from host count?**
One address = network ID (unusable)
One address = broadcast (unusable)
62 usable hosts in /26, not 64.

**Q: What is CIDR?**
Classless Inter-Domain Routing.
Replaces old classful system.
/24 means 24 bits for network,
8 bits for hosts = 254 usable hosts.

**Q: How many subnets from /24 using /26?**
Borrowed 2 bits = 2^2 = 4 subnets
Each with 62 usable hosts.

**Q: What is a broadcast address?**
Last IP in a subnet.
Packets sent here go to ALL hosts.
Cannot be assigned to a device.
