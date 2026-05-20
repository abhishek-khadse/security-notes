# Lecture 29: VLANs and Trunking
Date: May 20, 2026
Source: NetworkChuck Free CCNA

## What is a VLAN?
Logical segmentation of one physical network
into multiple isolated broadcast domains.
Same switch — completely separate networks.

## VLAN Example
Switch has 24 ports:
Ports 1-8   → VLAN 10 (HR)
Ports 9-16  → VLAN 20 (Finance)
Ports 17-24 → VLAN 30 (IT)
HR cannot talk to Finance
Finance cannot talk to IT
Even though same physical switch

## VLAN Types
| Type | Purpose |
|------|---------|
| Default (VLAN 1) | All ports start here — avoid using |
| Data VLAN | User traffic |
| Management VLAN | SSH, SNMP, switch admin |
| Native VLAN | Untagged trunk traffic |
| Voice VLAN | VoIP traffic isolation |

## Port Types
| Port | VLANs | Use |
|------|-------|-----|
| Access Port | 1 VLAN only | Connect PCs, printers |
| Trunk Port | Multiple VLANs | Connect switches, routers |

## 802.1Q Tagging
Normal Ethernet frame:
[Dest MAC][Src MAC][Type][Data][FCS]
802.1Q tagged frame (trunk):
[Dest MAC][Src MAC][802.1Q Tag][Type][Data][FCS]
↑
4-byte VLAN tag
contains VLAN ID

## Cisco Configuration
```bash
# Access port config
interface fastEthernet0/1
switchport mode access
switchport access vlan 10

# Trunk port config
interface g0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30
switchport trunk native vlan 99

# Disable DTP (security)
switchport nonegotiate

# Verify
show vlan brief
show interfaces trunk
show running-config
```

## Inter-VLAN Routing Methods
| Method | How | Speed |
|--------|-----|-------|
| Router-on-a-Stick | One router interface, subinterfaces | Slower |
| Layer 3 Switch | Routing built into switch | Faster |

### Router-on-a-Stick Config
```bash
interface g0/0.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0

interface g0/0.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
```

## Offensive Angle
| Attack | Description |
|--------|-------------|
| VLAN Hopping | Access VLANs you shouldn't reach |
| Double Tagging | Bypass VLAN isolation |
| DTP Abuse | Trick switch into trunk mode |
| Native VLAN attack | Exploit untagged traffic |
| Management VLAN access | Control network infrastructure |

### VLAN Hopping — Switch Spoofing
Attacker connects device to switch
Sends DTP packets pretending to be a switch
Switch auto-negotiates trunk link
Attacker now sees ALL VLANs
= Complete VLAN isolation bypass
Prevention:
switchport mode access        # Force access mode
switchport nonegotiate        # Disable DTP

### Double Tagging Attack
Works when native VLAN = VLAN 1 (default)
Attacker sends frame with 2 VLAN tags:
Outer tag: VLAN 1 (native, gets stripped)
Inner tag: VLAN 20 (target VLAN)
First switch strips outer tag (native)
Second switch sees inner tag = VLAN 20
Packet delivered to Finance VLAN
ONE WAY attack (no return traffic)
Prevention: Change native VLAN to unused VLAN

### DTP Exploitation
```bash
# Tool: Yersinia (VLAN attack tool)
yersinia -G    # GUI mode
# Select 802.1Q
# Launch DTP attack
# Switch negotiates trunk
# Attacker sees all VLANs

# Prevention:
switchport nonegotiate    # Disable DTP globally
```

### Management VLAN Attack
If attacker reaches management VLAN:

SSH to all switches
Telnet to all routers
SNMP query all devices
Change any configuration
= Complete network control

Always isolate management VLAN
Never put users on management VLAN

## SOC Detection
| Attack | Detection |
|--------|-----------|
| VLAN hopping | Unexpected cross-VLAN traffic |
| DTP abuse | Unexpected trunk negotiation |
| Double tagging | Malformed 802.1Q frames |
| Management VLAN access | Unauthorized SSH/SNMP |
| Native VLAN mismatch | Native VLAN inconsistency alert |

### Key SOC Rules
Rule 1: Cross-VLAN traffic without
inter-VLAN routing = hopping attack
Rule 2: New trunk link established = alert
Rule 3: DTP packet from access port = suspicious
Rule 4: Management VLAN access from
unknown device = critical
Rule 5: Double-tagged frames detected = attack
Rule 6: Native VLAN mismatch on trunk = misconfiguration

## VLAN Security Best Practices
Never use VLAN 1 for anything
Change native VLAN to unused VLAN (e.g. 999)
Disable DTP on all access ports
Restrict trunk allowed VLANs
Separate management VLAN
Disable unused switch ports
Put unused ports in "parking lot" VLAN

## Troubleshooting
| Problem | Cause | Fix |
|---------|-------|-----|
| No communication | Wrong VLAN assignment | Check access port VLAN |
| Trunk failure | Native VLAN mismatch | Match native VLAN both sides |
| VLAN missing | Not created | Create VLAN first |
| Inter-VLAN fails | No routing | Add router/L3 switch |

## Interview Questions
**Q: What is VLAN hopping?**
Attacker gains access to VLANs they
shouldn't reach. Two methods:
Switch spoofing via DTP, double tagging.
Prevention: disable DTP, change native VLAN.

**Q: Access port vs trunk port?**
Access = one VLAN, connects end devices.
Trunk = multiple VLANs, connects switches.
Tagged with 802.1Q on trunk links.

**Q: What is native VLAN?**
VLAN whose traffic travels untagged on trunk.
Default is VLAN 1 — security risk.
Change to unused VLAN to prevent attacks.

**Q: What is DTP?**
Dynamic Trunking Protocol.
Auto-negotiates trunk links between switches.
Security risk — disable with nonegotiate.
Attacker can spoof switch and get trunk access.

**Q: Why avoid VLAN 1?**
Default VLAN — all ports start here.
Native VLAN default = untagged traffic.
Double tagging attacks target VLAN 1.
Many management protocols use VLAN 1.
Change everything away from VLAN 1.
