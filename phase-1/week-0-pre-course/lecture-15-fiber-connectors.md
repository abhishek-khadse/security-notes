# Lecture 15: Fiber Connectors
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## Main Fiber Connectors
| Connector | Shape | Lock Type | Common Use |
|-----------|-------|-----------|------------|
| SC | Square | Push-pull | Telecom, data centers |
| LC | Small | Clip | SFP modules, switches |
| ST | Round | Twist-lock | Older networks |
| FC | Round | Threaded | Industrial |
| MPO/MTP | Multi-fiber | Push-pull | 40G/100G backbone |

## Polishing Types
| Type | Reflection | Typical Color |
|------|------------|---------------|
| PC | Medium | Beige |
| UPC | Low | Blue |
| APC | Very low | Green |

## APC vs UPC
APC means Angled Physical Contact.
The angled end face reduces back reflection.
It is often green and is used where low reflection is important.

UPC means Ultra Physical Contact.
It is usually blue and is common in many standard fiber links.

## Simplex vs Duplex
| Type | Direction | Use |
|------|-----------|-----|
| Simplex | One-way | Simple single-direction links |
| Duplex | Two-way | Most network links |

## Offensive Angle
| Attack | Description |
|--------|-------------|
| Physical Fiber Tap | Bend or split fiber to capture light |
| Wrong Connector Swap | Cause link failure or signal loss |
| Dirty Connector | Cause intermittent network issues |
| Data Center Access | Physical access to fiber routes and patch panels |

### Critical Safety Point
Never look directly into a fiber connector.
Laser light can cause permanent eye damage.
Use proper fiber inspection tools.

## SOC Detection
| Issue | Detection |
|-------|-----------|
| Fiber Tap | Optical power level drop |
| Physical Access | Data center access log alert |
| Link Failure | Interface down alert |
| Dirty Connector | CRC errors or signal loss alerts |

## Interview Questions
**Q: Which connector is commonly used in SFP modules?**
LC is commonly used because its small size fits SFP form factors well.

**Q: What is an APC connector?**
APC stands for Angled Physical Contact.
It has an angled end face, is usually green, and reduces back reflection.

**Q: Why should fiber connectors be cleaned?**
Dust blocks or scatters light.
Even tiny particles can cause signal loss, CRC errors, or unstable links.
Use a proper fiber cleaning tool before connecting fiber.
