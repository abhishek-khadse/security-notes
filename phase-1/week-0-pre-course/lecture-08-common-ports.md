# Lecture 08: Common Ports
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## What is a Port?
A port is a logical endpoint that identifies a service on a device.
IP address = device.
Port number = service on that device.

Example: `192.168.1.10:443` means HTTPS on host `192.168.1.10`.

## Port Ranges
| Range | Type |
|-------|------|
| 0-1023 | Well-known ports |
| 1024-49151 | Registered ports |
| 49152-65535 | Dynamic/private ports |

## Critical Ports - Must Memorize
| Port | Protocol | Service | Security Risk |
|------|----------|---------|---------------|
| 21 | TCP | FTP | Plain-text credentials |
| 22 | TCP | SSH | Brute force |
| 23 | TCP | Telnet | No encryption |
| 25 | TCP | SMTP | Email spoofing |
| 53 | TCP/UDP | DNS | Poisoning, tunneling |
| 80 | TCP | HTTP | Clear-text web traffic |
| 443 | TCP | HTTPS | TLS misconfig, encrypted C2 |
| 445 | TCP | SMB | EternalBlue, ransomware |
| 3389 | TCP | RDP | Brute force, BlueKeep |
| 3306 | TCP | MySQL | Database exposure |
| 1433 | TCP | MSSQL | Database attacks |
| 5432 | TCP | PostgreSQL | Database exposure |
| 389 | TCP/UDP | LDAP | AD enumeration |
| 636 | TCP | LDAPS | Directory service exposure |
| 161 | UDP | SNMP | Information leakage |

## Offensive Angle
### Port Scanning
```bash
# Quick scan top ports
nmap -sV --top-ports 1000 target.com

# Full TCP scan
nmap -sS -p- target.com

# Service version detection
nmap -sV -sC target.com

# UDP scan
nmap -sU --top-ports 100 target.com
```

### Dangerous Port Examples
| Port | Example Risk |
|------|--------------|
| 445 | EternalBlue -> WannaCry ransomware |
| 3389 | BlueKeep and RDP brute force |
| 23 | Telnet credential sniffing |
| 21 | Anonymous FTP or plain-text login |
| 161 | SNMP community string leaks network data |

## SOC Detection
| Activity | Alert |
|----------|-------|
| Sequential port hits | Port scan detected |
| Many ports from one IP | Recon activity |
| Port 23 traffic | Insecure Telnet usage |
| Port 445 spike | SMB attack or worm behavior |
| Port 3389 failures | RDP brute force |

## Interview Questions
**Q: Why is Telnet insecure?**
Telnet sends all data, including passwords, in clear text.
Anyone sniffing the network can read the session.

**Q: What makes port 445 dangerous?**
Port 445 is used by SMB.
SMB has been targeted by major exploits such as EternalBlue, which helped spread WannaCry ransomware.

**Q: What is the difference between port 80 and 443?**
Port 80 is HTTP and is unencrypted.
Port 443 is HTTPS and uses TLS encryption.
