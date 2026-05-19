# Lecture 12: Optical Fiber
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## What is Optical Fiber?
Optical fiber transmits data using light instead of electrical signals.
It is used for high speed, long distance, and low-interference network links.

## Fiber vs Copper
| Feature | Fiber | Copper |
|---------|-------|--------|
| Signal | Light | Electrical |
| Speed | Very high | Lower |
| Distance | Very long | Usually 100 m max for Ethernet |
| EMI | Immune | Affected |
| Security | Harder to tap | Easier to tap |
| Cost | Higher | Lower |

## Types of Optical Fiber
| Type | Core Size | Light Source | Distance | Common Use |
|------|-----------|--------------|----------|------------|
| Single Mode | Smaller | Laser | Long | ISP, telecom, long backbone |
| Multi Mode | Larger | LED/VCSEL | Shorter | Campus, data center |

## Fiber Connectors
| Connector | Shape | Common Use |
|-----------|-------|------------|
| SC | Square | Telecom |
| LC | Small | SFP modules |
| ST | Round | Older networks |
| FC | Threaded | Industrial |

## Fiber Standards
| Standard | Speed |
|----------|-------|
| Fast Ethernet Fiber | 100 Mbps |
| Gigabit Fiber | 1 Gbps |
| 10 Gigabit Fiber | 10 Gbps |
| 40G/100G | Data centers |

## Offensive Angle
| Attack | Description |
|--------|-------------|
| Physical Fiber Tap | Bending or splitting fiber to capture light |
| Optical Splitter | Inserted to copy optical signal |
| Fiber Cut | Physical denial-of-service attack |
| Data Center Access | Physical access to fiber routes and patch panels |

### Fiber Tapping Reality
Fiber is harder to tap than copper, but it is not impossible.
Tapping fiber often causes signal loss or attenuation that may be detectable.
Physical security of fiber routes is still critical.

## SOC Detection
| Threat | Detection |
|--------|-----------|
| Physical Tap | Signal loss or attenuation spike |
| Fiber Cut | Link-down alert |
| Unusual Traffic | Bandwidth anomaly on fiber links |

## Interview Questions
**Q: Why is fiber more secure than copper?**
Fiber is harder to tap physically.
Tapping fiber usually requires bending, splitting, or accessing the cable, which may cause detectable signal changes.
Fiber also does not leak electromagnetic signals like copper can.

**Q: What is the difference between single-mode and multi-mode fiber?**
Single-mode uses a smaller core and laser light for long distances.
Multi-mode uses a larger core for shorter distances and is often cheaper for campus or data center use.
