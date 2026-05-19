# Lecture 09: Networking Protocols
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## Protocol Quick Reference
| Protocol | Purpose | Port | Security Risk |
|----------|---------|------|---------------|
| ARP | IP -> MAC mapping | - | ARP spoofing |
| ICMP | Diagnostics and ping | - | Recon, flood |
| DHCP | Automatic IP assignment | 67/68 | Rogue DHCP |
| DNS | Domain -> IP lookup | 53 | Poisoning, tunneling |
| HTTP | Web traffic | 80 | Clear-text attacks |
| HTTPS | Secure web traffic | 443 | TLS misconfig, encrypted C2 |
| FTP | File transfer | 20/21 | Plain-text credentials |
| SFTP | Secure file transfer over SSH | 22 | Brute force risk |
| SSH | Secure remote access | 22 | Brute force |
| Telnet | Remote access | 23 | No encryption |
| SMTP | Send email | 25 | Spoofing, relay abuse |
| POP3 | Receive email | 110 | Plain-text if not encrypted |
| IMAP | Sync email | 143 | Plain-text if not encrypted |
| SNMP | Network management | 161/162 | Information leakage |
| LDAP | Directory service | 389 | AD enumeration |
| SMB | File sharing | 445 | Ransomware, lateral movement |
| RDP | Remote desktop | 3389 | Brute force |
| NTP | Time sync | 123 | Amplification attacks |

## Most Important for Cybersecurity

### ARP - Why It Matters
ARP has no built-in authentication.
An attacker can send a fake ARP reply saying, "I am the gateway."
If the victim believes it, traffic can flow through the attacker, causing a man-in-the-middle situation.

### DNS - Why It Matters
DNS usually has no encryption by default.
If DNS is poisoned, a victim may be redirected from a real domain to an attacker's IP address.
DNS tunneling can also hide command-and-control or data exfiltration inside DNS queries.

### SMB - Why It Matters
SMB runs on port 445 and is used for file sharing.
It is a major target for ransomware and lateral movement.
EternalBlue targeted SMB and helped spread WannaCry.

## Offensive Angle
```bash
# ARP spoofing
arpspoof -i eth0 -t 192.168.1.5 192.168.1.1

# DNS enumeration
dig any target.com
dnsenum target.com

# SMTP enumeration
nmap -p 25 --script smtp-enum-users target.com

# SNMP enumeration using default community string
snmpwalk -c public -v1 192.168.1.1
```

## SOC Detection
| Protocol | Attack | Detection |
|----------|--------|-----------|
| ARP | Spoofing | Gratuitous ARP or MAC/IP mismatch |
| DNS | Tunneling | High DNS query volume or long subdomains |
| SMB | Lateral movement | Port 445 anomaly and unusual file access |
| SNMP | Enumeration | Default community string usage |
| RDP | Brute force | Failed login spike |
| NTP | Amplification | NTP flood alert |

## Interview Questions
**Q: What is the difference between POP3 and IMAP?**
POP3 usually downloads emails to one device.
IMAP syncs emails across multiple devices.

**Q: Why is SSH better than Telnet?**
SSH encrypts the session.
Telnet sends everything in clear text.

**Q: What is DNS tunneling?**
DNS tunneling hides data inside DNS queries.
Attackers can encode data into subdomain names to bypass some network controls and exfiltrate information.
