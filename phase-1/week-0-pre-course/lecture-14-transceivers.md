# Lecture 14: Network Transceivers
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## Big Idea
A transceiver is a device that transmits and receives signals.

Transmitter + receiver = transceiver.

In networking, transceivers are commonly used in switches, routers, firewalls, and servers.
They allow devices to connect using fiber or copper links.

## Why Transceivers Matter
Network devices often have ports where you can insert modules.
These modules decide what type of cable and speed the port supports.

Example:

A switch may support SFP modules.
You can insert a fiber SFP to connect fiber, or a copper SFP to connect RJ45 copper.

## SFP
SFP means Small Form-factor Pluggable.

Common use:

- 1 Gbps links.
- Fiber connections.
- Sometimes copper connections.

Important feature:

SFP modules are hot-swappable.

## SFP+
SFP+ looks similar to SFP but supports higher speed.

Common use:

- 10 Gbps links.

Easy memory:

SFP = 1G.
SFP+ = 10G.

## QSFP and QSFP28
QSFP is used for higher-speed data center links.

Common speeds:

- QSFP = 40 Gbps.
- QSFP28 = 100 Gbps.

These are common in data centers and high-speed backbone networks.

## Single-Mode vs Multi-Mode Transceivers
Single-mode transceivers are used for long distance.
They usually use laser light.

Multi-mode transceivers are used for shorter distance.
They are common in data centers and campus networks.

Important:

The transceiver must match the fiber type and distance requirement.

## Hot-Swappable
Hot-swappable means you can replace the module while the device is running.

Why useful:

- Less downtime.
- Easier maintenance.
- Faster replacement during failures.

## Offensive Angle
Hardware can also be a security risk.

Possible attacks:

- Rogue transceiver inserted into a device.
- Malicious hardware implant.
- Swapping a legitimate module with a tapped one.
- Supply chain compromise before delivery.

These attacks require physical access or supply chain access, but they can be serious.

## SOC Detection
Watch for:

- Unsupported module alerts.
- Transceiver diagnostic warnings.
- Optical power level changes.
- Inventory mismatch.
- Physical access logs showing unauthorized activity.

## Interview Ready Answers
**What is SFP?**

SFP stands for Small Form-factor Pluggable.
It is a hot-swappable module used for fiber or copper network connections.

**What is the difference between SFP and SFP+?**

SFP commonly supports 1 Gbps.
SFP+ commonly supports 10 Gbps.

**What does hot-swappable mean?**

Hot-swappable means a module can be replaced while the device is still powered on and running.

## Quick Revision
- Transceiver = transmitter + receiver.
- SFP = usually 1G.
- SFP+ = usually 10G.
- QSFP = 40G.
- QSFP28 = 100G.
- Hot-swappable = replace without shutting down.
