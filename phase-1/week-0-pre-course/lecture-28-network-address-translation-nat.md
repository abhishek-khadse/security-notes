# Lecture 28: Network Address Translation (NAT)
Date: May 20, 2026
Source: NetworkChuck Free CCNA

## What is NAT?
Converts private IP ↔ public IP.
Allows multiple devices to share one public IP.
Solves IPv4 address exhaustion.

## NAT Types
| Type | Mapping | Use Case |
|------|---------|---------|
| Static NAT | 1 private → 1 public (fixed) | Servers, hosting |
| Dynamic NAT | 1 private → any from pool | Medium networks |
| PAT/Overload | Many private → 1 public + ports | Home/office (most common) |

## PAT Example (Most Common)
192.168.1.10 → 49.1.1.1:1025
192.168.1.11 → 49.1.1.1:1026
192.168.1.12 → 49.1.1.1:1027
All share ONE public IP.
Differentiated by port number.

## NAT Terminology
| Term | Meaning |
|------|---------|
| Inside Local | Your private IP (192.168.x.x) |
| Inside Global | Your public IP (49.x.x.x) |
| Outside Local | External server IP as seen inside |
| Outside Global | Real external server IP |

## NAT Process
PC (192.168.1.10)
↓
Router sees private IP
Replaces with public IP + port
Creates NAT table entry
↓
Internet (sees 49.1.1.1:1025)
↓
Response comes back to 49.1.1.1:1025
Router checks NAT table
Forwards to 192.168.1.10

## Port Forwarding
External user wants to reach internal server
Router has public IP 49.1.1.1
Port forward rule:
49.1.1.1:80 → 192.168.1.10:80
Now external users reach internal web server
Used for: web servers, CCTV, game servers

## Cisco NAT Config
```bash
# Static NAT
ip nat inside source static 192.168.1.10 49.1.1.10

# PAT (overload)
ip nat inside source list 1 interface g0/0 overload

# Mark interfaces
interface g0/0
ip nat outside

interface g0/1
ip nat inside

# Verify
show ip nat translations
show ip nat statistics
```

## Offensive Angle
| Attack | Description |
|--------|-------------|
| NAT bypass | Find real internal IPs behind NAT |
| Port forwarding abuse | Exposed internal services |
| NAT slipstreaming | Browser attack bypasses NAT |
| CGNAT issues | Multiple victims share one IP |
| Port scanning NAT | Find open forwarded ports |
| Session hijacking | Predict NAT port allocation |

### NAT Slipstreaming Attack
Browser-based attack bypasses NAT
Tricks router into forwarding attacker's traffic
to internal network devices
Works via ALG (Application Layer Gateway)
in router's NAT implementation
No user interaction needed after clicking link
Internal network exposed to attacker

### Finding Internal IPs Behind NAT
```bash
# Target has NAT — can't see internal IPs directly
# Methods to discover internal network:

# 1. WebRTC leak in browser
# Reveals real private IP even behind NAT

# 2. SSRF vulnerability in web app
curl "https://target.com/fetch?url=http://169.254.169.254/"
# Fetches cloud metadata = reveals internal IPs

# 3. Error messages
# Some apps leak 192.168.x.x in error responses

# 4. Email headers
# Sometimes contain internal IP of mail server
```

### Port Forwarding Enumeration
```bash
# Scan common forwarded ports
nmap -sV -p 80,443,8080,8443,3389,22,21 target.com

# These might be forwarded to internal servers:
# 3389 = RDP to internal Windows server
# 22   = SSH to internal Linux server
# 8080 = Internal web application
# 21   = FTP server

# Each = direct access to internal network
```

### CGNAT Challenges for Attackers AND Defenders
CGNAT = ISP puts multiple customers behind 1 IP
Attacker: can't target specific customer by IP
Defender: logs show same IP for many users
= difficult attribution in investigations
= "which customer did the attack?"

## SOC Detection
| Attack | Detection |
|--------|-----------|
| NAT slipstreaming | Unusual ALG traffic patterns |
| Port scan on public IP | Nmap scan detected by IDS |
| Port forward abuse | Unexpected inbound connections |
| Internal IP leak | Private IP in external logs |
| Session prediction | Sequential port allocation abuse |

### Key SOC Rules
Rule 1: Inbound connection to forwarded port
from blacklisted IP = alert
Rule 2: NAT table exhaustion = DoS attempt
Rule 3: Private IP appearing in external traffic
= configuration error or leak
Rule 4: Multiple failed connections to
forwarded port = brute force
Rule 5: Unusual ALG behavior = NAT slipstream

### Important: NAT ≠ Security
Common misconception:
"We're behind NAT so we're safe"

Reality:
NAT hides internal IPs — that's all
NAT does NOT:

Block malicious traffic
Detect attacks
Prevent data exfil
Stop outbound malware

Firewall + IDS + monitoring still required
NAT is NOT a security control

## Interview Questions
**Q: NAT vs PAT difference?**
NAT = each device needs own public IP.
PAT = many devices share one public IP
using different port numbers.
PAT is what home routers use.

**Q: Is NAT a firewall?**
No. NAT only hides internal IP addresses.
It does NOT filter traffic or detect attacks.
A separate firewall is always needed.

**Q: What is port forwarding?**
Directing inbound traffic from public IP+port
to specific internal server.
Example: 49.1.1.1:80 → 192.168.1.10:80
Allows external access to internal services.
Security risk if misconfigured.

**Q: Why is NAT unnecessary with IPv6?**
IPv6 has 340 undecillion addresses.
Every device gets unique public IPv6 address.
No need to share addresses = no NAT needed.
