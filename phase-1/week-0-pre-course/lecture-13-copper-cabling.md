# Lecture 13: Copper Cabling
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## Types of Copper Cables
| Type | Shielding | Use | Cost |
|------|-----------|-----|------|
| UTP | None | Standard LAN cabling | Low |
| STP | Shielded | High-interference areas | Medium |
| Coaxial | Shielded | TV, CCTV, older networks | Medium |

## Cable Categories
| Cable | Speed | Common Use |
|-------|-------|------------|
| Cat3 | 10 Mbps | Old phone and legacy Ethernet |
| Cat5 | 100 Mbps | Older networks |
| Cat5e | 1 Gbps | Home and small office networks |
| Cat6 | 10 Gbps at shorter distances | Modern office networks |
| Cat6a | 10 Gbps | Enterprise networks |
| Cat7 | 10+ Gbps | Shielded data center environments |

## Wiring Standards
### T568B (Common)
| Pin | Color |
|-----|-------|
| 1 | White-Orange |
| 2 | Orange |
| 3 | White-Green |
| 4 | Blue |
| 5 | White-Blue |
| 6 | Green |
| 7 | White-Brown |
| 8 | Brown |

## Cable Types by Use
| Cable | Connects | Example |
|-------|----------|---------|
| Straight-through | Different device types | PC -> Switch |
| Crossover | Same device types | PC -> PC |

Modern devices often support auto-MDI/MDIX, so crossover cables are less commonly needed today.

## Transmission Modes
| Mode | Direction |
|------|-----------|
| Simplex | One-way only |
| Half Duplex | Both ways, one at a time |
| Full Duplex | Both ways simultaneously |

## Offensive Angle
| Attack | Description |
|--------|-------------|
| Physical Tap | Device reads electrical signal |
| Crosstalk Exploitation | Attempt to observe adjacent cable signals |
| Patch Panel Access | Plug in rogue device |
| RJ45 Port Abuse | Connect unauthorized device to network |

### Physical Attack Scenario
1. Attacker gets physical access to an office.
2. Attacker finds an unlocked patch panel.
3. Attacker plugs in a small device or network tap.
4. The device captures traffic or provides remote access.

## SOC Detection
| Attack | Detection |
|--------|-----------|
| Rogue Device | New unknown MAC alert |
| Physical Tap | Signal quality degradation |
| Unauthorized Port | Port security violation |
| Patch Panel Abuse | Physical access control logs |

## Interview Questions
**Q: What is the maximum Ethernet cable length for copper?**
The common maximum distance for copper Ethernet is 100 meters.
Beyond that, signal quality degrades.
Fiber is used for longer distances.

**Q: What is the difference between UTP and STP?**
UTP has no shielding and is cheap and common.
STP has shielding, which helps protect against electromagnetic interference.
