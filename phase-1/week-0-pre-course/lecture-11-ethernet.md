# Lecture 11: Ethernet Standards
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## Big Idea
Ethernet is the most common wired LAN technology.
It defines how devices send data over cables inside local networks.

When you plug a PC into a switch using an RJ45 cable, you are usually using Ethernet.

## Ethernet Naming
Ethernet names look confusing at first, but they are easy to break down.

Example:

```text
100Base-T
```

Meaning:

- `100` = speed in Mbps.
- `Base` = baseband signaling.
- `T` = twisted-pair copper cable.

Another example:

```text
1000Base-T
```

This means 1 Gbps Ethernet over twisted-pair copper.

## Common Ethernet Standards
You do not need to memorize every standard immediately.
Start with the common ones.

### 10Base-T
Old Ethernet.
Speed is 10 Mbps.
Uses Cat3 cable.

### 100Base-TX
Fast Ethernet.
Speed is 100 Mbps.
Uses Cat5 cable.

### 1000Base-T
Gigabit Ethernet.
Speed is 1 Gbps.
Uses Cat5e or Cat6.

### 10GBase-T
10 Gigabit Ethernet.
Speed is 10 Gbps.
Uses Cat6a or better for full 100 m distance.

## Cable Categories
Easy way to remember:

- Cat5 = old.
- Cat5e = good for 1 Gbps.
- Cat6 = better, can support 10 Gbps at shorter distance.
- Cat6a = 10 Gbps up to 100 m.

## Ethernet Frame
Ethernet sends data in frames.

A frame contains:

- Destination MAC address.
- Source MAC address.
- Type or length field.
- Payload data.
- FCS for error checking.

Simple meaning:

Ethernet frames are local network delivery envelopes.
They use MAC addresses, not IP addresses, for local delivery.

## Hub vs Switch
A hub repeats traffic to everyone.
A switch sends traffic only where it needs to go.

This is why switches are faster and more secure than hubs.

Hub:

- Layer 1.
- Half-duplex.
- Everyone sees traffic.
- Rare today.

Switch:

- Layer 2.
- Full-duplex.
- Uses MAC address table.
- Standard today.

## CSMA/CD
CSMA/CD means Carrier Sense Multiple Access with Collision Detection.

It was used in old hub-based Ethernet.
Devices checked if the line was free before sending.
If two devices sent at the same time, a collision happened.

Modern switched Ethernet mostly eliminated collisions.

## Offensive Angle
Attackers may target Ethernet behavior.

Examples:

- MAC spoofing.
- CAM table overflow.
- VLAN hopping.
- Rogue device connected to switch port.

### CAM Table Overflow
A switch stores MAC addresses in a CAM table.
If an attacker floods the switch with many fake MAC addresses, the table may fill up.

In some cases, the switch may start flooding traffic like a hub.

Example tool:

```bash
macof -i eth0
```

## SOC Detection
Watch for:

- Many MAC addresses on one port.
- Same IP with different MAC addresses.
- Port security violations.
- Double-tagged VLAN frames.
- Unknown device connected to switch port.

## Interview Ready Answers
**Why are switches better than hubs?**

Switches forward traffic only to the correct destination using MAC addresses.
Hubs repeat traffic to everyone.
Switches are faster, more efficient, and more secure.

**What is CSMA/CD?**

CSMA/CD is an old Ethernet collision detection method used with hubs.
Modern switches mostly removed the need for it.

## Quick Revision
- Ethernet = wired LAN technology.
- Ethernet uses frames.
- Frames use MAC addresses.
- Hub = repeats to everyone.
- Switch = forwards using MAC table.
- 1000Base-T = Gigabit Ethernet.
