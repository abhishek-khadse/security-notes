# Lecture 31: Spanning Tree Protocol (STP)
Date: May 20, 2026
Source: NetworkChuck Free CCNA

## Why STP Exists?
Redundant switch links = good for reliability
BUT create switching loops = catastrophic

## Problems Without STP
| Problem | Description |
|---------|-------------|
| Broadcast storm | Frames loop forever, network dies |
| MAC instability | Switch keeps relearning MACs |
| Duplicate frames | Same frame received multiple times |

## How STP Works
Elect Root Bridge (lowest priority + MAC)
Every switch finds best path to root
Block redundant paths
Network is loop-free

## STP Terminology
| Term | Meaning |
|------|---------|
| Root Bridge | Central reference switch |
| BPDU | STP control messages |
| Root Port | Best path toward root |
| Designated Port | Forwarding port per segment |
| Blocked Port | Disabled to prevent loop |

## Root Bridge Election
Bridge ID = Priority + MAC Address
Default priority = 32768
Lowest Bridge ID wins

Example:
Switch A: Priority 32768 + MAC 00:AA:AA:AA:AA:AA
Switch B: Priority 32768 + MAC 00:BB:BB:BB:BB:BB
Switch C: Priority 4096  + MAC 00:CC:CC:CC:CC:CC
Switch C wins = Root Bridge (lowest priority)

## STP Port States
| State | Forwards? | Learns MACs? | Duration |
|-------|-----------|--------------|---------|
| Blocking | ❌ | ❌ | 20 sec |
| Listening | ❌ | ❌ | 15 sec |
| Learning | ❌ | ✅ | 15 sec |
| Forwarding | ✅ | ✅ | Normal |
| Disabled | ❌ | ❌ | Manual |

## STP Timers
| Timer | Default | Purpose |
|-------|---------|---------|
| Hello | 2 sec | BPDU interval |
| Forward Delay | 15 sec | Listening + Learning |
| Max Age | 20 sec | BPDU timeout |

## Port Cost (Path Selection)
| Speed | Cost |
|-------|------|
| 10 Mbps | 100 |
| 100 Mbps | 19 |
| 1 Gbps | 4 |
| 10 Gbps | 2 |

## STP Versions
| Version | Standard | Feature |
|---------|----------|---------|
| STP | 802.1D | Original, slow (30-50 sec) |
| RSTP | 802.1w | Fast convergence (<1 sec) |
| MSTP | 802.1s | Per-VLAN optimization |
| PVST+ | Cisco | STP per VLAN |

## Security Features
| Feature | Purpose | Config |
|---------|---------|--------|
| PortFast | Instant forwarding for PCs | spanning-tree portfast |
| BPDU Guard | Block rogue switches on PortFast ports | spanning-tree bpduguard enable |
| Root Guard | Prevent unauthorized root election | spanning-tree guard root |
| Loop Guard | Prevent unexpected loops | spanning-tree guard loop |

## Cisco Configuration
```bash
# Set switch as root bridge
spanning-tree vlan 10 priority 4096

# Enable PortFast on access port
interface fa0/1
spanning-tree portfast

# Enable BPDU Guard
interface fa0/1
spanning-tree bpduguard enable

# Verify STP
show spanning-tree
show spanning-tree root
show spanning-tree interface fa0/1
```

## Offensive Angle
| Attack | Description |
|--------|-------------|
| BPDU spoofing | Become root bridge = see all traffic |
| STP manipulation | Change topology = MITM |
| BPDU flood | DoS via STP recalculation |
| Rogue switch | Connect switch = trigger STP election |

### Root Bridge Takeover — Powerful Attack
Attacker connects rogue switch
Sends BPDU with priority 0
Lowest priority = wins election
Attacker's switch becomes Root Bridge
Now ALL traffic flows through
attacker's switch to reach root
= Passive MITM on entire Layer 2 network
= See everything, modify anything
Tool: Yersinia
yersinia stp -attack 1   # Become root bridge

### STP DoS Attack
Send continuous BPDU topology changes
Forces constant STP recalculation
Network unstable — keeps converging
During convergence = no traffic forwarded
= Network outage
Prevention: BPDU Guard + Root Guard

### Why This Matters in Enterprise
Most enterprise networks have redundant links
STP running on all switches
One rogue switch = complete L2 takeover
Attacker sees ALL unencrypted traffic
Financial data, credentials, PII
Hardest part: just needs physical access
Plug in device = attack starts automatically

## SOC Detection
| Attack | Detection |
|--------|-----------|
| Root bridge change | Unexpected root bridge alert |
| BPDU from access port | BPDU Guard violation |
| Topology change spike | Rapid TCN messages |
| New switch connected | Unknown device on network |
| Broadcast storm | Traffic spike + CPU spike |

### Key SOC Rules
Rule 1: Root bridge changes unexpectedly = critical
Rule 2: BPDU received on PortFast port = violation
Rule 3: >10 topology changes/min = attack or loop
Rule 4: New switch MAC on trunk port = investigate
Rule 5: Broadcast traffic >30% of bandwidth = storm
Rule 6: STP recalculation during business hours = alert

## Troubleshooting
| Problem | Cause | Fix |
|---------|-------|-----|
| Broadcast storm | STP failure/loop | Check topology, fix links |
| Root bridge changed | Rogue switch | Enable Root Guard |
| Slow convergence | Using old STP | Upgrade to RSTP |
| Port stuck in blocking | Misconfiguration | Check port costs |

## Interview Questions
**Q: What is a broadcast storm?**
Switching loop causes broadcast frames
to circulate endlessly.
Network becomes unusable.
CPU on all switches maxes out.
STP prevents by blocking redundant paths.

**Q: How is Root Bridge elected?**
Lowest Bridge Priority wins.
Tie = lowest MAC address wins.
Default priority = 32768.
Set manually: priority 4096 for desired root.

**Q: STP vs RSTP?**
STP = 802.1D, 30-50 seconds convergence.
RSTP = 802.1w, sub-second convergence.
RSTP has additional port roles:
Alternate + Backup ports for faster recovery.
Always use RSTP in modern networks.

**Q: What is BPDU Guard?**
Protects PortFast-enabled access ports.
If BPDU received = port immediately shutdown.
Prevents rogue switches connecting to
access ports and hijacking STP topology.
