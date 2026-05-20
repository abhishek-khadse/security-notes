# Lecture 26: Static Routing
Date: May 20, 2026
Source: NetworkChuck Free CCNA

## What is Static Routing?
Routes manually configured by admin.
Router does NOT learn routes automatically.
Fixed paths — don't change unless admin changes them.

## Static vs Dynamic Routing
| Feature | Static | Dynamic |
|---------|--------|---------|
| Configuration | Manual | Automatic |
| Scale | Small networks | Large networks |
| Overhead | Very low | Higher |
| Failover | Manual | Automatic |
| Security | No route broadcasts | Route advertisements |
| Use case | Stub networks, small office | Enterprise, ISP |

## Static Route Syntax
```bash
# Cisco IOS
ip route <destination> <mask> <next-hop>

# Example: to reach 192.168.2.0/24, send to 10.0.0.2
ip route 192.168.2.0 255.255.255.0 10.0.0.2

# Default route (catch-all)
ip route 0.0.0.0 0.0.0.0 192.168.1.1

# Floating static (backup route, AD=5)
ip route 192.168.2.0 255.255.255.0 10.0.0.2 5

# Null route (drop traffic)
ip route 192.168.5.0 255.255.255.0 null0
```

## Types of Static Routes
| Type | Purpose | Example |
|------|---------|---------|
| Standard | Fixed path to network | ip route 192.168.2.0... |
| Default | Unknown destinations | ip route 0.0.0.0 0.0.0.0... |
| Floating | Backup route higher AD | ...10.0.0.2 5 |
| Null | Drop unwanted traffic | ...null0 |
| Summary | Combine multiple routes | Supernet |

## Network Example
```
LAN1 (192.168.1.0/24)
        ↓
Router A (10.0.0.1)
        ↓ /30 link
Router B (10.0.0.2)
        ↓
LAN2 (192.168.2.0/24)
```

Router A config:
ip route 192.168.2.0 255.255.255.0 10.0.0.2

Router B config:
ip route 192.168.1.0 255.255.255.0 10.0.0.1

## Administrative Distance (AD)
Lower AD = more trusted route

| Route Source | AD |
|--------------|-----|
| Connected | 0 |
| Static | 1 |
| EIGRP | 90 |
| OSPF | 110 |
| RIP | 120 |

Static AD=1 = very trusted
Floating static uses higher AD as backup

## Routing Table Commands
```bash
# Cisco
show ip route
show running-config
ping 192.168.2.1
traceroute 192.168.2.1

# Linux
ip route show
ip route add 192.168.2.0/24 via 10.0.0.2
route -n

# Windows
route print
route add 192.168.2.0 mask 255.255.255.0 10.0.0.2
tracert 192.168.2.1
```

## Offensive Angle
| Attack | Description |
|--------|-------------|
| Route manipulation | Change static route = redirect all traffic |
| Default route hijack | Change default gateway = intercept everything |
| Null route DoS | Route victim traffic to null0 = black hole |
| Routing loop | Misconfigure routes = network outage |
| Router access | Default credentials on router = change routes |

### Route Manipulation Attack
Attacker gains access to router
(default creds, CVE, physical access)
Changes default route:
ip route 0.0.0.0 0.0.0.0 [attacker-IP]
Now ALL traffic flows through attacker
= Complete network MITM
= Intercept all communications
= Modify traffic in transit

### Black Hole Attack
Attacker adds null route for victim subnet:
ip route 192.168.1.0 255.255.255.0 null0
All traffic to 192.168.1.0/24 = dropped
= Silent DoS — no error messages
= Difficult to diagnose
= Network appears up but unreachable

### Reconnaissance via Routing Table
```bash
# After compromising a router/server
# Routing table reveals entire network map

show ip route          # Cisco
ip route show          # Linux
route print            # Windows

# See all connected networks
# Plan lateral movement paths
# Identify high-value targets
```

### Default Credentials on Routers
Many routers ship with:
admin/admin
admin/password
cisco/cisco
admin/<blank>
Attacker finds exposed router
Tries default creds
Gets in → changes routes
= Full network compromise
Tools: Routersploit
routersploit> use scanners/routers/router_scan

## SOC Detection
| Attack | Detection |
|--------|-----------|
| Route change | Config change alert on router |
| New static route | SIEM: routing table modified |
| Default route change | Critical alert — affects all traffic |
| Null route added | Connectivity loss to specific subnet |
| Router login | Failed/successful auth alert |

### Key SOC Rules for Routing
Rule 1: Routing table change outside
change window = critical alert
Rule 2: New default route = immediate alert
Rule 3: Failed router login attempts = brute force
Rule 4: Null route added = black hole DoS
Rule 5: Route added pointing to unknown IP = MITM
Rule 6: Config download from router = data exfil

## Troubleshooting Static Routes
| Problem | Cause | Fix |
|---------|-------|-----|
| No connectivity | Wrong next hop IP | Verify next hop reachable |
| Route missing | Typo in config | Check show ip route |
| Ping fails | Interface down | Check interface status |
| Routing loop | Incorrect return route | Verify both directions |

## Interview Questions
**Q: When to use static vs dynamic routing?**
Static = small networks, stub networks,
security-sensitive environments.
Dynamic = large networks, enterprise,
where automatic failover needed.

**Q: What is a floating static route?**
Backup route with higher AD than primary.
Primary fails → floating route activates.
Example: primary AD=1, floating AD=5.

**Q: What is a null route used for?**
Drop unwanted traffic silently.
Used for:
- Block traffic to specific networks
- Prevent routing loops
- Traffic black-holing

**Q: What is administrative distance?**
Trustworthiness rating of route source.
Lower = more trusted.
Static = 1 (very trusted).
Router prefers lower AD when
multiple routes to same destination.
