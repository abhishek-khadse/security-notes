# Lecture 04: Networking Functions
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## Main Networking Functions
| Function | Purpose | Example |
|----------|---------|---------|
| Communication | Device-to-device data exchange | Email, VoIP, chat |
| Resource Sharing | Share hardware or software resources | Printer, file share |
| Data Transfer | Move data between systems | File download |
| Remote Access | Access systems remotely | SSH, RDP, VPN |
| Centralized Management | Manage users and systems from one place | Active Directory |
| Security | Protect systems and traffic | Firewall, IDS, MFA |
| Scalability | Grow without major redesign | Add servers or links |
| Reliability | Keep services available | Redundant links |
| Internet Access | Connect internal users to the internet | Router, ISP |
| Traffic Management | Control and optimize traffic flow | QoS, load balancer |

## Key Protocols per Function
| Function | Protocols |
|----------|-----------|
| Communication | HTTP, SMTP, SIP |
| File Transfer | FTP, SFTP, SMB |
| Remote Access | SSH, RDP, VPN |
| Security | TLS, IPsec |
| Management | SNMP, LDAP, Kerberos |

## Offensive Angle
| Function | Example Attack |
|----------|----------------|
| Remote Access | RDP brute force, SSH brute force |
| Centralized Management | Active Directory attacks |
| Resource Sharing | SMB exploits, printer attacks |
| Traffic Management | QoS abuse, traffic flooding |
| Internet Access | DNS abuse, proxy bypass |

### Remote Access Attack Example
```bash
# SSH brute force with Hydra
hydra -l admin -P wordlist.txt ssh://192.168.1.1

# RDP brute force
hydra -l administrator -P wordlist.txt rdp://192.168.1.1
```

## SOC Detection
| Function | Detection |
|----------|-----------|
| Remote Access | Multiple failed SSH/RDP logins |
| Centralized Management | Unusual AD admin activity |
| Resource Sharing | SMB traffic spike or unusual access |
| Internet Access | DNS anomaly, unusual destination |
| Traffic Management | Unexpected bandwidth or QoS changes |

## Interview Questions
**Q: What is Active Directory?**
Active Directory is Microsoft's centralized identity and access management system for Windows networks.
It manages users, computers, groups, authentication, and policies.

**Q: Why is SSH preferred over Telnet?**
SSH encrypts the session.
Telnet sends data, including passwords, in clear text.
