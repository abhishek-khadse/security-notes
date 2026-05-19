# Lecture 03: OSI Networking Devices
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## Big Idea
Different network devices work at different OSI layers.
To understand a device, ask one question:

**What information does this device use to make decisions?**

If it uses electrical signals, it is Layer 1.
If it uses MAC addresses, it is Layer 2.
If it uses IP addresses, it is Layer 3.
If it filters applications or web requests, it may work at higher layers.

## Hub
A hub is a Layer 1 device.
It does not understand MAC addresses or IP addresses.

Simple meaning:

Whatever comes in one port is repeated out all other ports.

Security problem:

Anyone connected to a hub can see a lot of traffic.
This is why hubs are almost never used in modern networks.

## Switch
A switch is mainly a Layer 2 device.
It uses MAC addresses to forward frames.

Simple meaning:

A switch learns which MAC address is connected to which port.
Then it sends traffic only where it needs to go.

Security issues:

- ARP spoofing
- MAC flooding
- VLAN hopping
- Port security bypass

## Router
A router is a Layer 3 device.
It uses IP addresses to move packets between networks.

Simple meaning:

Switches connect devices inside a network.
Routers connect different networks together.

Security issues:

- Weak router passwords.
- Exposed admin panels.
- Route poisoning.
- Misconfigured access control lists.

## Firewall
A firewall filters traffic based on rules.
Depending on the firewall type, it can work from Layer 3 up to Layer 7.

Examples:

- Basic firewall: source IP, destination IP, port.
- Next-generation firewall: application, user, content, threat signatures.

Security role:

- Block unwanted traffic.
- Log suspicious connections.
- Enforce network policy.

## Proxy
A proxy is usually a Layer 7 device.
It sits between the client and destination service.

Common uses:

- Web filtering.
- Logging.
- Caching.
- Access control.
- Hiding internal client details.

Security risk:

If a proxy is misconfigured, attackers may use it to bypass restrictions or hide activity.

## IDS and IPS
IDS means Intrusion Detection System.
It detects suspicious traffic and alerts.

IPS means Intrusion Prevention System.
It detects suspicious traffic and can block it.

Easy memory:

- IDS = sees and alerts.
- IPS = sees and stops.

## WAF
WAF means Web Application Firewall.
It protects web applications at Layer 7.

It looks for attacks like:

- SQL injection.
- XSS.
- Malicious HTTP requests.
- Suspicious user input.

## SOC Analyst View
Device logs are very important.

Useful logs:

- Firewall logs show allowed and blocked traffic.
- Proxy logs show websites and URLs.
- IDS/IPS logs show attack signatures.
- Router logs show route and traffic anomalies.
- Switch logs show MAC changes, port security, and VLAN events.

## Interview Ready Answers
**What is the difference between a hub, switch, and router?**

A hub repeats traffic to all ports.
A switch forwards traffic using MAC addresses.
A router forwards traffic between networks using IP addresses.

**What is the difference between IDS and IPS?**

IDS detects and alerts.
IPS detects and blocks.

## Quick Revision
- Hub = Layer 1 = repeats signals.
- Switch = Layer 2 = MAC address.
- Router = Layer 3 = IP address.
- Proxy = Layer 7 = application requests.
- WAF = Layer 7 = web app protection.
