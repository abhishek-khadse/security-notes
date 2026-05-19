# Lecture 13: Copper Cabling
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## Big Idea
Copper cabling carries data using electrical signals.
It is common in homes, offices, and LAN environments.

Most Ethernet cables you see with RJ45 connectors are copper twisted-pair cables.

## UTP
UTP means Unshielded Twisted Pair.

Simple meaning:

The wires are twisted together, but there is no extra shielding.

Good for:

- Homes.
- Offices.
- Normal LAN cabling.

Why common:

- Cheap.
- Easy to install.
- Good enough for most environments.

## STP
STP means Shielded Twisted Pair.

Simple meaning:

It has shielding to reduce electromagnetic interference.

Good for:

- Industrial areas.
- High-interference environments.
- Places near heavy electrical equipment.

## Coaxial Cable
Coaxial cable has a central conductor and shielding around it.

Common uses:

- TV.
- CCTV.
- Cable internet.
- Older networks.

## Cable Categories
Think of categories as cable performance levels.

Important ones:

- Cat3: old, around 10 Mbps.
- Cat5: older Ethernet, around 100 Mbps.
- Cat5e: common for 1 Gbps.
- Cat6: better, supports 10 Gbps at shorter distance.
- Cat6a: supports 10 Gbps up to 100 m.

Easy memory:

Higher category usually means better speed and performance.

## Maximum Copper Ethernet Distance
The common Ethernet limit for copper is 100 meters.

If distance is more than 100 meters, use:

- Fiber.
- Another switch.
- Proper network extension design.

## T568B Wiring
T568B is a common wiring standard.

Pin order:

1. White-Orange
2. Orange
3. White-Green
4. Blue
5. White-Blue
6. Green
7. White-Brown
8. Brown

You do not need to memorize this perfectly at first, but recognize that wiring order matters.

## Straight-Through vs Crossover
Straight-through cable connects different device types.

Example:

- PC to switch.

Crossover cable connects same device types.

Example:

- PC to PC.
- Switch to switch.

Modern devices usually support auto-MDI/MDIX, so crossover cables are less needed today.

## Duplex
Simplex:

- One-way only.

Half-duplex:

- Both directions, but one at a time.

Full-duplex:

- Both directions at the same time.

Modern switched Ethernet usually uses full-duplex.

## Offensive Angle
Physical access is dangerous.

Possible attacks:

- Plugging a rogue device into an RJ45 port.
- Accessing an unlocked patch panel.
- Installing a physical network tap.
- Connecting a small device for remote access.

Example scenario:

An attacker enters an office, finds an unused network port, connects a small device, and later accesses the internal network remotely.

## SOC Detection
Watch for:

- New unknown MAC address.
- Port security violation.
- Link-up event on unused port.
- Device connected outside business hours.
- Signal quality degradation.

## Interview Ready Answers
**What is the maximum copper Ethernet distance?**

The common maximum distance is 100 meters.
Beyond that, signal quality degrades.

**What is the difference between UTP and STP?**

UTP has no shielding and is common in normal offices.
STP has shielding and is used where electromagnetic interference is a concern.

## Quick Revision
- Copper uses electrical signals.
- UTP = common and cheap.
- STP = shielded.
- Cat5e = 1 Gbps.
- Cat6a = 10 Gbps up to 100 m.
- Copper Ethernet distance = 100 m.
