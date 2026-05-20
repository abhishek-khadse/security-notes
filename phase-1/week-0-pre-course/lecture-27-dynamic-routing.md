# Lecture 27: Dynamic Routing
Date: May 20, 2026
Source: NetworkChuck Free CCNA

## What is Dynamic Routing?
Routers automatically learn + update routes
using routing protocols.
Routers talk to each other — share routing tables.

## Routing Protocols Comparison
| Protocol | Type | Metric | AD | Use Case |
|----------|------|--------|----|---------|
| RIP | Distance Vector | Hop count | 120 | Small networks |
| OSPF | Link State | Cost | 110 | Enterprise |
| EIGRP | Hybrid | BW + Delay | 90 | Cisco networks |
| BGP | Path Vector | AS Path | 20/200 | Internet/ISPs |

## Protocol Types
| Type | How it works | Example |
|------|-------------|---------|
| Distance Vector | Share routing table periodically | RIP |
| Link State | Build complete network map | OSPF |
| Hybrid | Combines both | EIGRP |
| Path Vector | Track AS path | BGP |

## Administrative Distance
Lower AD = more trusted

Connected    = 0
Static       = 1
EIGRP        = 90
OSPF         = 110
RIP          = 120

When 2 routes exist to same destination
Router picks LOWER AD

## RIP
Distance vector, hop count metric
Max 15 hops (16 = unreachable)
RIPv1 = classful (old)
RIPv2 = classless (use this)
Slow convergence = major weakness

Config:
```text
router rip
version 2
network 192.168.1.0
```

## OSPF
Link state, Cost metric
Uses SPF (Dijkstra) algorithm
Requires Area 0 (backbone)
Fast convergence
Scalable for large networks

Config:
```text
router ospf 1
network 192.168.1.0 0.0.0.255 area 0
```

## BGP
Path vector protocol
Used between ISPs + on internet backbone
Metric = AS Path (autonomous system hops)
Most critical protocol on internet
BGP hijacking = redirect internet traffic

## Key Concepts
| Term | Meaning |
|------|---------|
| Convergence | Time for all routers to agree on routes |
| Routing loop | Packets circulate endlessly |
| Split horizon | Don't send route back where you learned it |
| Adjacency | Neighbor relationship between routers |

## Commands
```bash
# View routing table
show ip route          # Cisco
ip route show          # Linux
route print            # Windows

# Check protocols
show ip protocols
show ip ospf neighbor
show ip eigrp neighbors

# Troubleshoot
ping 192.168.2.1
traceroute 192.168.2.1
```

## Offensive Angle
| Attack | Protocol | Description |
|--------|----------|-------------|
| Route poisoning | RIP | Inject fake routes |
| BGP hijacking | BGP | Redirect internet traffic |
| OSPF injection | OSPF | Inject false LSAs |
| Rogue router | Any | Advertise false routes |
| Routing loop | Any | Create network outage |

### BGP Hijacking — Most Dangerous
BGP controls internet routing
No authentication by default (historically)

Attack:
ISP/attacker announces ownership of IP block
Other routers believe it = update routes
Traffic for that IP block → attacker

Real examples:

2010: China Telecom hijacked 15% of internet
2018: Amazon Route 53 hijacked via BGP
2019: European traffic routed through China

Impact:

Steal traffic
Intercept communications
Redirect financial transactions
Nation-state surveillance

### OSPF LSA Injection
OSPF uses Link State Advertisements
No authentication by default
Attacker on network sends fake LSA:

Claims to be a router
Advertises false routes
All routers update their tables
Traffic redirected to attacker

Tool: Loki (OSPF injection tool)

### RIP Route Poisoning
RIP accepts route updates from anyone
Attacker sends fake RIP update:
"Destination X is unreachable (16 hops)"
All RIP routers remove that route
= DoS for that destination

Or:
"I have best path to 0.0.0.0/0"
= Becomes default gateway for network
= All traffic through attacker

### Rogue Router Attack
```bash
# Attacker connects device to network
# Runs routing protocol
# Advertises routes

# For OSPF attack:
# Install quagga/FRRouting on Kali
# Configure as OSPF router
# Advertise false routes
# All OSPF routers update tables
```

## SOC Detection
| Attack | Detection |
|--------|-----------|
| BGP hijacking | Unexpected BGP route announcements |
| OSPF injection | New LSA from unknown router |
| RIP poisoning | Unusual RIP updates |
| Rogue router | New routing neighbor alert |
| Routing loop | TTL exceeded errors spike |
| Route flapping | Rapid route add/remove |

### Key SOC Rules
Rule 1: New BGP peer established = verify immediately
Rule 2: Unexpected route announcement = alert
Rule 3: New OSPF neighbor = investigate
Rule 4: Default route changed via protocol = critical
Rule 5: TTL exceeded errors spike = routing loop
Rule 6: Same prefix from 2 sources = hijack attempt

### Defense: Enable Routing Authentication
```bash
# OSPF MD5 authentication
interface GigabitEthernet0/0
ip ospf authentication message-digest
ip ospf message-digest-key 1 md5 SecurePass123

# BGP defense: RPKI (Route Origin Validation)
# Cryptographically verify BGP announcements
# Prevents BGP hijacking
```

## Interview Questions
**Q: RIP vs OSPF key difference?**
RIP = distance vector, hop count, max 15 hops,
slow convergence, simple config.
OSPF = link state, cost metric, unlimited hops,
fast convergence, complex config.
OSPF preferred in enterprise.

**Q: What is BGP hijacking?**
Attacker announces ownership of IP prefixes
they don't own. Other BGP routers believe it
and route traffic to attacker.
Can redirect entire countries' internet traffic.
Defense: RPKI validates route origins.

**Q: What is convergence?**
Time for all routers to agree on network topology
after a change. Faster convergence = better.
OSPF converges faster than RIP.

**Q: Why is BGP critical?**
BGP is the routing protocol of the internet.
Every ISP uses BGP to exchange routes.
BGP hijacking = redirect global internet traffic.
Most impactful routing attack possible.
