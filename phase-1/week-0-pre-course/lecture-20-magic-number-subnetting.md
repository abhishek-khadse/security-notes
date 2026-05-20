# Lecture 20: Magic Number Subnetting
Date: May 20, 2026
Source: NetworkChuck Free CCNA

## What is Magic Number Subnetting?
Fastest subnetting method — no binary needed.
Used in interviews, CCNA, cybersecurity labs.

## The Formula
Magic Number = 256 - interesting octet
Interesting octet = where mask changes from 255

## Magic Number Cheat Sheet
| CIDR | Mask | Magic Number | Hosts | Subnets from /24 |
|------|------|--------------|-------|------------------|
| /25 | .128 | 128 | 126 | 2 |
| /26 | .192 | 64 | 62 | 4 |
| /27 | .224 | 32 | 30 | 8 |
| /28 | .240 | 16 | 14 | 16 |
| /29 | .248 | 8 | 6 | 32 |
| /30 | .252 | 4 | 2 | 64 |

## 3-Step Magic Number Method
Step 1: Find magic number (256 - mask)
Step 2: List subnets (0, magic, magic*2...)
Step 3: Find which subnet your IP falls in

## Example 1 — 192.168.1.70/26
Step 1: Magic = 256 - 192 = 64
Step 2: Subnets = 0, 64, 128, 192
Step 3: 70 falls between 64-127
Network:   192.168.1.64
First Host: 192.168.1.65
Last Host:  192.168.1.126
Broadcast:  192.168.1.127

## Example 2 — /27
Magic = 256 - 224 = 32
Subnets: .0, .32, .64, .96, .128, .160, .192, .224
Hosts per subnet = 2^5 - 2 = 30

## Example 3 — /28
Magic = 256 - 240 = 16
Subnets: .0, .16, .32, .48, .64...
Hosts = 2^4 - 2 = 14

## Speed Rules
Memorize magic numbers: 128,64,32,16,8,4
List subnets by incrementing magic number
Network = first in block
Broadcast = (next subnet) - 1
Hosts = network+1 to broadcast-1

## Offensive Angle
| Use | How |
|-----|-----|
| Fast mental subnetting | Calculate target ranges instantly during recon |
| Find valid hosts quickly | Avoid scanning network/broadcast addresses |
| Identify subnet boundaries | Find where one network ends, another begins |
| Plan scan ranges | nmap -sn 192.168.1.64/26 |

### Attacker Uses Magic Numbers Too
```bash
# Found internal IP: 10.10.15.70/27
# Quick mental calc:
# Magic = 32, subnet = 10.10.15.64
# Range = 10.10.15.64 - 10.10.15.95
# Scan exactly that range:

nmap -sV 10.10.15.64/27

# Found IP: 172.16.5.200/28
# Magic = 16, falls in .192 block
# Range = .192 - .207
# Scan:
nmap -sV 172.16.5.192/28
```

### HTB/CTF Subnetting
In CTF labs you often get:

One IP in a subnet
Need to find other hosts
Magic number = instant subnet range
Scan exactly that range
Find hidden services

## SOC Detection
| Scenario | Alert |
|----------|-------|
| Full subnet scan | All IPs in /26 hit sequentially |
| Targeted subnet scan | Specific /28 block scanned |
| Cross-boundary traffic | Traffic from .64/26 to .128/26 |
| New host in subnet | Unknown IP appears in range |

### Practical SOC Scenario
Alert fires: Sequential scan 192.168.1.64-126
Analyst checks: That's a /26 subnet
Conclusion: Attacker enumerated subnet
Action: Block source IP, investigate
what was found in that subnet
