# Lecture 14: Network Transceivers
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## What is a Transceiver?
A transceiver is a device that both transmits and receives signals.
Transmitter + receiver = transceiver.

Network transceivers are commonly used in switches, routers, and servers to connect copper or fiber links.

## SFP Types
| Type | Speed | Common Use |
|------|-------|------------|
| SFP | 1 Gbps | Standard fiber or copper |
| SFP+ | 10 Gbps | High-speed fiber or DAC |
| QSFP | 40 Gbps | Data centers |
| QSFP28 | 100 Gbps | Hyperscale data centers |

## Fiber Transceiver Types
| Type | Distance | Light Source |
|------|----------|--------------|
| Single Mode | Long | Laser |
| Multi Mode | Shorter | LED/VCSEL |

## Key Feature: Hot-Swappable
SFP modules are hot-swappable.
They can be replaced without shutting down the switch or router, which helps maintain uptime.

## Offensive Angle
| Attack | Description |
|--------|-------------|
| Rogue SFP | Malicious or unauthorized module |
| Physical Access | Swap a legitimate SFP with a tapped one |
| Supply Chain | Compromised module before delivery |
| Hardware Implant | Backdoor hidden in network hardware |

### Supply Chain Attack Reality
Advanced attackers may target network hardware before it reaches the organization.
Hardware backdoors can be difficult to detect with software alone.
Inventory control, trusted suppliers, and physical inspection are important.

## SOC Detection
| Issue | Detection |
|-------|-----------|
| Rogue Hardware | Inventory audit mismatch |
| Signal Anomaly | Transceiver diagnostic alerts |
| Unauthorized Swap | Physical security logs |
| Unsupported Module | Switch or router compatibility alerts |

## Interview Questions
**Q: What is SFP?**
SFP stands for Small Form-factor Pluggable.
It is a hot-swappable module used in switches and routers for fiber or copper connectivity.

**Q: What is the difference between SFP and SFP+?**
SFP commonly supports 1 Gbps.
SFP+ commonly supports 10 Gbps with a similar form factor.

**Q: What does hot-swappable mean?**
Hot-swappable means a module can be replaced while the device is still running.
