# Lecture 18: IPv4 Subnet Masks
Date: May 20, 2026
Source: NetworkChuck Free CCNA

## What is a Subnet Mask?
Divides IP address into network + host portion.
Devices use it to determine:
- Am I on same network as destination?
- Do I need to go through gateway?

## Binary Logic
IP:   192.168.1.10  = 11000000.10101000.00000001.00001010
Mask: 255.255.255.0 = 11111111.11111111.11111111.00000000
AND:  192.168.1.0   = 11000000.10101000.00000001.00000000
↑ Network ID
1 = network bit | 0 = host bit

## CIDR Quick Reference
| CIDR | Subnet Mask | Hosts | Block Size |
|------|-------------|-------|------------|
| /8 | 255.0.0.0 | 16M | 16M |
| /16 | 255.255.0.0 | 65534 | 65536 |
| /24 | 255.255.255.0 | 254 | 256 |
| /25 | 255.255.255.128 | 126 | 128 |
| /26 | 255.255.255.192 | 62 | 64 |
| /27 | 255.255.255.224 | 30 | 32 |
| /28 | 255.255.255.240 | 14 | 16 |
| /29 | 255.255.255.248 | 6 | 8 |
| /30 | 255.255.255.252 | 2 | 4 |

## Formulas
Hosts    = 2^h - 2     (h = host bits)
Subnets  = 2^n         (n = borrowed bits)
Block    = 256 - last octet of mask
Network  = IP AND Mask
Broadcast = Last IP in block

## /26 Example — 192.168.1.0/24 → 4 subnets
Borrow 2 bits → /26
Mask = 255.255.255.192
Block = 256 - 192 = 64
Subnet 1: 192.168.1.0   → .63   (hosts: .1-.62)
Subnet 2: 192.168.1.64  → .127  (hosts: .65-.126)
Subnet 3: 192.168.1.128 → .191  (hosts: .129-.190)
Subnet 4: 192.168.1.192 → .255  (hosts: .193-.254)

## Block Size Cheat Sheet
| Last Octet | Block Size |
|------------|------------|
| 128 | 128 |
| 192 | 64 |
| 224 | 32 |
| 240 | 16 |
| 248 | 8 |
| 252 | 4 |

## Offensive Angle
| Concept | Attack Use |
|---------|------------|
| Subnet discovery | Map target network ranges |
| /30 subnets | Point-to-point links = only 2 hosts |
| Broadcast address | Smurf attack amplification |
| Network segmentation | Plan pivot routes between subnets |
| CIDR miscalculation | Admins expose more IPs than intended |

### Network Recon Using Subnets
```bash
# Identify target subnet
nmap -sn 192.168.1.0/24

# Scan specific subnet range
nmap -sn 10.0.0.0/8

# Find all open ports in subnet
nmap -sV 192.168.1.0/24

# After compromise — map internal subnets
ip route show          # Linux
route print            # Windows
arp -a                 # ARP table
```

### Subnet Misconfiguration Attack
Admin sets up /23 instead of /24
Accidentally exposes 512 IPs instead of 256
Attacker discovers extra subnet
Finds unmonitored devices there
Classic mistake in large enterprises

## SOC Detection
| Attack | Detection |
|--------|-----------|
| Subnet scan | Sequential IP sweep alert |
| Smurf attack | ICMP broadcast flood alert |
| Cross-subnet traffic | VLAN boundary violation |
| /30 link attack | Point-to-point link compromise |

### Key SOC Rules
Rule 1: Traffic crossing subnet boundary
without authorization = alert
Rule 2: ICMP to broadcast address = Smurf
Rule 3: Sequential scan across /24 = recon
Rule 4: New device in subnet = investigate

## Interview Questions
**Q: What is subnet mask in binary?**
1s represent network bits.
0s represent host bits.
AND operation with IP = network address.

**Q: How many hosts in /27?**
Host bits = 32 - 27 = 5
Hosts = 2^5 - 2 = 30 usable hosts.

**Q: What is block size?**
256 minus last octet of subnet mask.
/26 mask = 255.255.255.192
Block = 256 - 192 = 64
Subnets increment by 64.

**Q: Why does /30 only have 2 hosts?**
Host bits = 2
Hosts = 2^2 - 2 = 2
Used for point-to-point WAN links.
Router to router connections.
