# Lecture 11: Ethernet Standards
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## Ethernet Naming Format
Example: `100Base-T`

- `100` = speed in Mbps.
- `Base` = baseband signaling.
- `T` = twisted-pair copper cable.

## Ethernet Standards
| Standard | Speed | Cable | Max Distance |
|----------|-------|-------|--------------|
| 10Base-T | 10 Mbps | Cat3 | 100 m |
| 100Base-TX | 100 Mbps | Cat5 | 100 m |
| 1000Base-T | 1 Gbps | Cat5e/Cat6 | 100 m |
| 10GBase-T | 10 Gbps | Cat6a/Cat7 | 100 m |

## Cable Categories
| Cable | Max Speed | Common Use |
|-------|-----------|------------|
| Cat5 | 100 Mbps | Older networks |
| Cat5e | 1 Gbps | Home and small office networks |
| Cat6 | 10 Gbps at shorter distances | Office networks |
| Cat6a | 10 Gbps | Enterprise networks |
| Cat7 | 10+ Gbps | Data centers and shielded environments |

## Ethernet Frame Structure
| Field | Purpose |
|-------|---------|
| Destination MAC | Where to send the frame |
| Source MAC | Who sent the frame |
| Type / Length | Indicates payload type or length |
| Data | Actual payload |
| FCS | Error checking |

## Hub vs Switch
| Feature | Hub | Switch |
|---------|-----|--------|
| Layer | 1 | 2 |
| Traffic | Repeats to all ports | Sends to target MAC |
| Duplex | Half-duplex | Full-duplex |
| Security | Low | Better |
| Used Today | Rare | Standard |

## Offensive Angle
| Concept | Example Attack |
|---------|----------------|
| Hub Network | Passive sniffing sees all traffic |
| Switch Network | CAM overflow can force flooding behavior |
| MAC Address | Spoofing to bypass simple controls |
| Ethernet Frames | Frame injection |

### CAM Table Overflow Attack
```bash
# Tool: macof (Kali Linux)
macof -i eth0

# Floods switch with fake MACs.
# If the CAM table fills up, the switch may flood traffic.
```

## SOC Detection
| Attack | Detection |
|--------|-----------|
| CAM Overflow | MAC flood alert in switch logs |
| MAC Spoofing | Same IP using different MAC addresses |
| Packet Sniffing | Promiscuous mode checks where available |
| VLAN Hopping | Double-tagged frame detection |

## Interview Questions
**Q: What is CSMA/CD?**
Carrier Sense Multiple Access with Collision Detection.
It was used in old hub-based Ethernet.
Devices checked whether the line was free before sending.
Modern switched Ethernet mostly eliminated collisions.

**Q: Why are switches better than hubs?**
Switches send traffic only to the intended device.
Hubs repeat traffic to everyone.
Switches also support full-duplex communication and are more efficient.
