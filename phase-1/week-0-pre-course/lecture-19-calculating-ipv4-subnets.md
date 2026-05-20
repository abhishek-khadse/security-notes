# Lecture 19: Calculating IPv4 Subnets and Hosts
Date: May 20, 2026
Source: NetworkChuck Free CCNA

## The 7-Step Subnetting Process
1. How many subnets needed?
2. Calculate borrowed bits: 2^n ≥ subnets
3. Add borrowed bits to CIDR
4. Calculate new subnet mask
5. Block size = 256 - last mask octet
6. List all subnets by incrementing block size
7. Find host range for each subnet

## Master Formula Sheet
Subnets  = 2^n          n = borrowed bits
Hosts    = 2^h - 2      h = remaining host bits
Block    = 256 - mask   last octet only
Network  = first IP in block
Broadcast = last IP in block
Hosts    = everything between network + broadcast

## Example 1 — /24 into 4 subnets
Given: 192.168.1.0/24, need 4 subnets

| Step | Result |
|------|--------|
| 1 | Need 4 subnets |
| 2 | 2^n ≥ 4 → n=2 bits borrowed |
| 3 | /24 + 2 = /26 |
| 4 | Mask = 255.255.255.192 |
| 5 | Block = 256 - 192 = 64 |
| 7 | Hosts = 2^6 - 2 = 62 per subnet |

| Subnet | Network | Host Range | Broadcast |
|--------|---------|------------|-----------|
| 1 | 192.168.1.0 | .1 - .62 | .63 |
| 2 | 192.168.1.64 | .65 - .126 | .127 |
| 3 | 192.168.1.128 | .129 - .190 | .191 |
| 4 | 192.168.1.192 | .193 - .254 | .255 |

## Example 2 — /27
Mask = 255.255.255.224
Block = 256 - 224 = 32
Hosts = 2^5 - 2 = 30

| Subnet | Network | Broadcast |
|--------|---------|-----------|
| 1 | .0 | .31 |
| 2 | .32 | .63 |
| 3 | .64 | .95 |
| 4 | .96 | .127 |

## Full CIDR Cheat Sheet
| CIDR | Mask | Block | Subnets from /24 | Hosts |
|------|------|-------|------------------|-------|
| /24 | .0 | 256 | 1 | 254 |
| /25 | .128 | 128 | 2 | 126 |
| /26 | .192 | 64 | 4 | 62 |
| /27 | .224 | 32 | 8 | 30 |
| /28 | .240 | 16 | 16 | 14 |
| /29 | .248 | 8 | 32 | 6 |
| /30 | .252 | 4 | 64 | 2 |

## Common Mistakes
| Mistake | Why Wrong | Fix |
|---------|-----------|-----|
| Forgetting -2 | Network + broadcast unusable | Always subtract 2 |
| Using .0 as host | That's network address | Start from .1 |
| Using .255 as host | That's broadcast | End at .254 |
| Wrong block size | Wrong subnet ranges | 256 - mask |

## Offensive Angle
| Concept | Attack Use |
|---------|-----------|
| /30 subnet | Only 2 hosts = point-to-point link between routers |
| Large /8 | Massive scan space for internal recon |
| Subnet boundaries | Find unmonitored segments |
| Broadcast address | Smurf DDoS amplification |
| Miscalculated subnets | Exposed IPs admin didn't know existed |

### Subnetting in Real Attacks
```bash
# Step 1: Find target subnet range
nmap -sn 192.168.0.0/16

# Step 2: Identify all live subnets
nmap -sn 10.0.0.0/8

# Step 3: After internal compromise
# Find ALL connected subnets
ip route show
route print        # Windows
cat /etc/hosts

# Step 4: Pivot to new subnet
# Use compromised host as gateway
# Lateral movement across subnets
```

### Why /30 Matters to Attackers
/30 = only 2 usable hosts
Used for router-to-router WAN links
Compromising /30 link =
intercept ALL traffic between sites
= massive impact, small target

## SOC Detection
| Scenario | Detection Rule |
|----------|---------------|
| Subnet scan | >50 IPs hit in sequence from 1 source |
| Cross-subnet pivot | Traffic between isolated VLANs |
| Broadcast flood | ICMP to .255 address = Smurf |
| New subnet activity | Unknown subnet traffic alert |
| /30 link compromise | Unusual traffic on WAN link |

## Subnetting Quick Practice
**Q: How many hosts in /28?**
Host bits = 32-28 = 4
Hosts = 2^4 - 2 = 14 ✓

**Q: Block size of /27?**
Mask = 224, Block = 256-224 = 32 ✓

**Q: 3rd subnet of 10.0.0.0/27?**
Block = 32
Subnet 1 = 10.0.0.0
Subnet 2 = 10.0.0.32
Subnet 3 = 10.0.0.64 ✓

## Interview Questions
**Q: Given 172.16.0.0/16, create 8 subnets.**
Need 8 → 2^3 = 8 → borrow 3 bits
New CIDR = /19
Mask = 255.255.224.0
Block = 256 - 224 = 32 (third octet)
Subnet 1: 172.16.0.0/19
Subnet 2: 172.16.32.0/19
Subnet 3: 172.16.64.0/19
...etc

**Q: Why is /30 used for WAN links?**
Only 2 usable hosts needed.
One for each end of point-to-point link.
Wastes minimum IP space.

**Q: What is the host range of 192.168.1.128/26?**
Block = 64
Network = 192.168.1.128
Broadcast = 192.168.1.191
Hosts = 192.168.1.129 - 192.168.1.190
