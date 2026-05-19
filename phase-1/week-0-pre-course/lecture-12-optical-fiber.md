# Lecture 12: Optical Fiber
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## Big Idea
Optical fiber sends data as light.
Copper sends data as electrical signals.

Fiber is used when we need:

- Higher speed.
- Longer distance.
- Less interference.
- Better backbone connectivity.

## Why Fiber is Useful
Fiber has major advantages:

- It can carry data over long distances.
- It supports very high speeds.
- It is immune to electromagnetic interference.
- It is harder to tap than copper.

But fiber also has disadvantages:

- It is more expensive.
- It is more delicate.
- It needs special connectors and transceivers.
- Cleaning and handling matter a lot.

## Single-Mode Fiber
Single-mode fiber has a smaller core.
It usually uses laser light.

Best for:

- Long distance.
- ISP networks.
- Telecom.
- Building-to-building links.
- Long backbone links.

Simple memory:

Single-mode = single path = long distance.

## Multi-Mode Fiber
Multi-mode fiber has a larger core.
It is usually used for shorter distances.

Best for:

- Data centers.
- Campus networks.
- Shorter building links.

Simple memory:

Multi-mode = multiple light paths = shorter distance.

## Fiber vs Copper
Copper is common and cheap.
Fiber is faster and better for distance.

Copper is usually limited to 100 meters for Ethernet.
Fiber can go much farther depending on the type and transceiver.

Copper can be affected by EMI.
Fiber is immune to EMI because it uses light.

## Fiber Connectors
Common connectors:

- SC: square connector, common in telecom.
- LC: small connector, common with SFP modules.
- ST: round connector, older networks.
- FC: threaded connector, industrial use.

Most important for modern networking:

Remember LC because it is commonly used with SFP modules.

## Offensive Angle
Fiber is harder to attack than copper, but not impossible.

Possible attacks:

- Physical fiber tap.
- Optical splitter.
- Fiber cut.
- Accessing patch panels in data centers.

Fiber tapping can cause signal loss.
This is why monitoring optical power levels matters.

## SOC Detection
Watch for:

- Link-down alerts.
- Optical power level drop.
- Sudden attenuation increase.
- Bandwidth anomaly on backbone links.
- Physical access to fiber patch panels.

## Interview Ready Answers
**Why is fiber more secure than copper?**

Fiber is harder to tap physically and does not leak electromagnetic signals like copper.
Tapping fiber usually requires physical access and may cause detectable signal loss.

**What is the difference between single-mode and multi-mode fiber?**

Single-mode uses a smaller core and is used for long distances.
Multi-mode uses a larger core and is used for shorter distances like data centers or campus networks.

## Quick Revision
- Fiber uses light.
- Copper uses electricity.
- Single-mode = long distance.
- Multi-mode = shorter distance.
- LC connector is common with SFP.
- Fiber cuts cause link-down alerts.
