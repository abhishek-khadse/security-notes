# Lecture 01: OSI Model
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## What is the OSI Model?
The OSI model is a 7-layer framework that explains how data moves across a network.
It helps standardize communication between systems and makes troubleshooting easier.

For cybersecurity, every attack and every detection point can be mapped to one or more OSI layers.

## Layers Quick Reference
| Layer | Name | Data Unit | Key Devices | Protocols / Examples |
|-------|------|-----------|-------------|----------------------|
| 7 | Application | Data | Proxy, web server, WAF | HTTP, FTP, DNS, SMTP |
| 6 | Presentation | Data | - | TLS, SSL, JPEG, encryption |
| 5 | Session | Data | - | NetBIOS, RPC, session control |
| 4 | Transport | Segment / Datagram | Firewall, load balancer | TCP, UDP |
| 3 | Network | Packet | Router, Layer 3 switch | IP, ICMP, IPsec |
| 2 | Data Link | Frame | Switch, bridge | Ethernet, ARP, MAC |
| 1 | Physical | Bits | Cable, hub, repeater | Copper, fiber, radio |

## Memory Trick
**All People Seem To Need Data Processing**

Top to bottom: Application -> Physical

## Offensive Angle
| Layer | Example Attack |
|-------|----------------|
| 1 | Physical tap, cable cut, USB drop |
| 2 | ARP spoofing, MAC flooding, VLAN hopping |
| 3 | IP spoofing, ICMP recon, routing attacks |
| 4 | SYN flood, TCP/UDP port scanning |
| 5 | Session hijacking |
| 6 | TLS downgrade, encoding bypass |
| 7 | SQL injection, XSS, phishing, DNS abuse |

## SOC Detection
| Layer | Example Detection |
|-------|-------------------|
| 1 | New device alert, link-down event |
| 2 | ARP anomaly, MAC address change |
| 3 | Port scan alert, unusual ICMP traffic |
| 4 | SYN flood rate alert |
| 5 | Concurrent session anomaly |
| 6 | TLS downgrade or certificate anomaly |
| 7 | WAF alert, DNS anomaly, suspicious login |

## Interview Questions
**Q: What is the OSI model?**
The OSI model is a 7-layer framework that standardizes and explains network communication.
Each layer has specific functions, protocols, devices, and security risks.

**Q: Which layer does a switch operate on?**
A switch mainly operates at Layer 2, the Data Link layer, using MAC addresses.

**Q: Which layer does a router operate on?**
A router operates at Layer 3, the Network layer, using IP addresses.
