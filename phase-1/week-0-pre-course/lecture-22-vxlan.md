# Lecture 22: VXLAN (Virtual Extensible LAN)
Date: May 20, 2026
Source: NetworkChuck Free CCNA

## What is VXLAN?
Extends Layer 2 networks over Layer 3 infrastructure.
Solves VLAN scalability problem in cloud/data centers.

## VLAN vs VXLAN
| Feature | VLAN | VXLAN |
|---------|------|-------|
| ID bits | 12-bit | 24-bit |
| Max networks | 4,096 | 16,777,216 |
| Layer | Layer 2 only | L2 over L3 |
| Use case | Enterprise LAN | Cloud/DC |
| Port | - | UDP 4789 |

## Key Components
| Component | Purpose |
|-----------|---------|
| VNI | VXLAN Network Identifier (24-bit) |
| VTEP | Encapsulates/decapsulates VXLAN traffic |
| Overlay | Virtual network on top |
| Underlay | Physical IP network underneath |
| UDP 4789 | Default VXLAN transport port |

## VXLAN Packet Structure
```
Original Ethernet Frame
        ↓
VXLAN Header (VNI)
        ↓
UDP Header (port 4789)
        ↓
IP Header
        ↓
Outer Ethernet Header
= Packet travels across IP network
```

## VXLAN Communication Flow
```
VM A → VTEP A → [encapsulate] →
IP Network → [decapsulate] →
VTEP B → VM B
```

## Spine-Leaf Architecture
```
VM → Leaf Switch → Spine Switch
              → Spine Switch
              → Leaf Switch → VM
```

Benefits:

- Every leaf = equal distance to spine
- Low latency
- High scalability
- Common in modern data centers

## Offensive Angle
| Attack | Description |
|--------|-------------|
| VTEP compromise | Control tunnel = intercept all overlay traffic |
| VXLAN flooding | Flood MAC table of VTEP |
| Tenant escape | Misconfigured VNI = access other tenants |
| UDP 4789 | Unencrypted VXLAN = packet sniffing |
| EVPN BGP attacks | Inject malicious routes into BGP |

### VXLAN Traffic Interception
VXLAN uses UDP — no encryption by default
Attacker on underlay network can:

Capture all UDP 4789 traffic
Decapsulate VXLAN headers
Read ALL overlay network traffic
Bypass tenant isolation

Tools:

Wireshark: filter udp.port==4789
Scapy: craft malicious VXLAN packets

### Tenant Escape Attack
Cloud provider uses VXLAN for tenant isolation
VNI 1000 = Tenant A
VNI 2000 = Tenant B
Misconfigured VTEP:

Accepts packets from wrong VNI
Tenant A accesses Tenant B traffic
Complete isolation bypass
Critical in multi-tenant clouds

### VTEP Discovery
```bash
# Find VXLAN VTEPs on network
nmap -sU -p 4789 192.168.1.0/24

# Capture VXLAN traffic in Wireshark
# Filter: udp.port == 4789

# With Scapy - craft VXLAN packet
from scapy.all import *
vxlan_pkt = Ether()/IP()/UDP(dport=4789)/VXLAN(vni=1000)/Ether()/IP()
send(vxlan_pkt)
```

## SOC Detection
| Attack | Detection |
|--------|-----------|
| VTEP compromise | Unusual tunnel endpoint activity |
| VNI scanning | Multiple VNI values tested |
| Tenant escape | Cross-VNI traffic detected |
| VXLAN flood | Mass UDP 4789 traffic |
| Rogue VTEP | Unknown VTEP IP appears |

### SOC Monitoring Rules
Rule 1: Unknown VTEP IP = rogue tunnel
Rule 2: Cross-VNI traffic = tenant escape attempt
Rule 3: Mass UDP 4789 = VXLAN flood
Rule 4: VTEP config change = unauthorized modification
Rule 5: BGP route injection to EVPN = routing attack

## Interview Questions
**Q: Why was VXLAN created?**
VLANs limited to 4096 networks.
Modern cloud needs millions of isolated networks.
VXLAN provides 16+ million VNIs.
Extends Layer 2 over existing Layer 3 infrastructure.

**Q: What is VTEP?**
VXLAN Tunnel Endpoint.
Encapsulates outgoing frames with VXLAN header.
Decapsulates incoming VXLAN packets.
Can be physical switch, router, or hypervisor.

**Q: What port does VXLAN use?**
UDP port 4789.
No built-in encryption — security risk.

**Q: VXLAN vs VLAN key difference?**
VLAN = 4096 max, Layer 2 only.
VXLAN = 16M+ networks, L2 over L3.
VXLAN scales for cloud, VLAN does not.

**Q: What is VNI?**
VXLAN Network Identifier.
24-bit value identifying virtual network.
Like VLAN ID but much larger scale.
