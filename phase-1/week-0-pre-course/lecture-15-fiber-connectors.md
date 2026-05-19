# Lecture 15: Fiber Connectors
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## Big Idea
Fiber connectors attach fiber cables to network devices, patch panels, and transceivers.

Fiber is sensitive.
A dirty, damaged, or wrong connector can cause signal loss and link problems.

## LC Connector
LC is small and very common in modern networking.

Where you see it:

- SFP modules.
- Switches.
- Routers.
- Data centers.

Easy memory:

LC = little connector.

## SC Connector
SC is square and larger than LC.

Where you see it:

- Telecom.
- Older fiber installations.
- Patch panels.

Easy memory:

SC = square connector.

## ST Connector
ST is round and uses a twist-lock style.

Where you see it:

- Older networks.
- Some legacy fiber installations.

## FC Connector
FC is round and threaded.

Where you see it:

- Industrial environments.
- Specialized fiber links.

## MPO/MTP Connector
MPO/MTP connectors carry multiple fibers in one connector.

Where you see it:

- Data centers.
- 40G and 100G backbone links.
- High-density fiber environments.

## Polishing Types
Fiber connector ends are polished in different ways.
The polish affects reflection and signal quality.

### PC
PC means Physical Contact.
It has medium reflection.

### UPC
UPC means Ultra Physical Contact.
It has lower reflection than PC.
It is usually blue.

### APC
APC means Angled Physical Contact.
It has very low back reflection.
It is usually green.

Important:

APC reduces reflection.
It is not simply about "more speed"; it is about better signal quality where reflection matters.

## Simplex vs Duplex
Simplex uses one fiber path.

Duplex uses two fiber paths:

- One for transmit.
- One for receive.

Most network links use duplex.

## Cleaning Fiber Connectors
Fiber connectors must be clean.

Even tiny dust can cause:

- Signal loss.
- CRC errors.
- Intermittent links.
- Complete link failure.

Always clean and inspect fiber before connecting.

## Safety Warning
Never look directly into a fiber connector.
Laser light can damage your eyes.

Use proper inspection tools.

## Offensive Angle
Physical access to fiber is dangerous.

Possible attacks:

- Fiber tapping.
- Wrong connector swap.
- Dirty connector causing instability.
- Fiber cut causing denial of service.
- Unauthorized access to data center patch panels.

## SOC Detection
Watch for:

- Optical power drop.
- Link-down alert.
- CRC errors.
- Signal loss.
- Physical access logs.
- Repeated fiber interface flapping.

## Interview Ready Answers
**Which connector is commonly used with SFP modules?**

LC connector is commonly used because it is small and fits SFP modules well.

**What is APC?**

APC means Angled Physical Contact.
It has an angled end face that reduces back reflection.
It is usually green.

**Why should fiber connectors be cleaned?**

Dust can block or scatter light.
Even tiny particles can cause signal loss or unstable links.

## Quick Revision
- LC = small, common with SFP.
- SC = square.
- ST = round twist-lock.
- APC = green, angled, low reflection.
- UPC = blue, low reflection.
- Clean fiber before connecting.
