# Lecture 03: OSI Networking Devices
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## Devices by Layer
| Device | Layer | Function |
|--------|-------|----------|
| Hub | 1 | Repeats bits to all ports |
| Switch | 2 | Forwards frames using MAC addresses |
| Router | 3 | Routes packets between networks |
| Firewall | 3-7 | Filters traffic based on policy |
| Proxy | 7 | Intermediary for client requests |
| IDS/IPS | 3-7 | Detects or prevents suspicious traffic |
| WAF | 7 | Protects web applications |

## Hub vs Switch vs Router
| Feature | Hub | Switch | Router |
|---------|-----|--------|--------|
| OSI Layer | 1 | 2 | 3 |
| Address Used | None | MAC address | IP address |
| Traffic Behavior | Sends to all ports | Sends to target MAC | Sends between networks |
| Security | Low | Better | Policy-dependent |

## Offensive Angle
| Device | Example Attack |
|--------|----------------|
| Hub | Passive sniffing because all traffic is repeated |
| Switch | CAM table overflow, ARP spoofing |
| Router | Default credentials, route poisoning |
| Firewall | Rule bypass, allowed-port abuse |
| Proxy | Credential interception, proxy misconfiguration |

## SOC Detection
| Device | Detection Role |
|--------|----------------|
| Firewall | Blocks and logs suspicious traffic |
| IDS/IPS | Detects signatures and anomalies |
| Proxy | Logs web requests and destinations |
| Router | Shows routing changes and traffic anomalies |
| Switch | Shows port security, MAC changes, VLAN events |

## Interview Questions
**Q: What is the difference between IDS and IPS?**
IDS detects and alerts.
IPS detects and can block automatically.

**Q: What is a proxy server?**
A proxy server is an intermediary between a client and another service.
It can be used for caching, filtering, logging, access control, and anonymity.
