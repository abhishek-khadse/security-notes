# Lecture 08: Common Ports
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## Big Idea
A port identifies a service running on a device.

Simple formula:

```text
IP address = device
Port number = service
```

Example:

```text
192.168.1.10:443
```

This means HTTPS service on device `192.168.1.10`.

## Port Ranges
You do not need to memorize every port range deeply at first.
Just understand the idea.

- `0-1023` = well-known ports.
- `1024-49151` = registered ports.
- `49152-65535` = dynamic/private ports.

Well-known ports are the most important for beginners and SOC work.

## Ports You Must Know First
### Port 21 - FTP
FTP is used for file transfer.

Security problem:

FTP can send usernames and passwords in clear text.

### Port 22 - SSH
SSH is used for secure remote login.

Security problem:

Attackers often brute force SSH.

### Port 23 - Telnet
Telnet is old remote login.

Security problem:

Telnet is not encrypted.
Avoid it.

### Port 25 - SMTP
SMTP is used to send email.

Security problem:

Can be abused for spoofing or open relay attacks.

### Port 53 - DNS
DNS converts domain names to IP addresses.

Security problem:

DNS can be abused for poisoning, tunneling, and command-and-control.

### Port 80 - HTTP
HTTP is web traffic without encryption.

Security problem:

Data can be read or modified if not protected.

### Port 443 - HTTPS
HTTPS is encrypted web traffic using TLS.

Security problem:

Attackers also use HTTPS to hide phishing, malware traffic, and command-and-control.

### Port 445 - SMB
SMB is used for Windows file sharing.

Security problem:

SMB is heavily targeted for ransomware and lateral movement.
EternalBlue attacked SMB and helped spread WannaCry.

### Port 3389 - RDP
RDP is used for Windows remote desktop.

Security problem:

Attackers commonly brute force RDP or use stolen credentials.

### Database Ports
Common database ports:

- MySQL = 3306
- MSSQL = 1433
- PostgreSQL = 5432

Security problem:

Databases should not be exposed directly to the internet unless there is a strong reason and strong protection.

### Directory and Management Ports
Important ports:

- LDAP = 389
- LDAPS = 636
- SNMP = 161

Security problem:

These can leak sensitive network or identity information if misconfigured.

## How to Remember Ports
Group them by purpose:

Remote access:

- SSH 22
- Telnet 23
- RDP 3389

Web:

- HTTP 80
- HTTPS 443

File sharing:

- FTP 21
- SMB 445

Name and identity:

- DNS 53
- LDAP 389
- LDAPS 636

Databases:

- MySQL 3306
- MSSQL 1433
- PostgreSQL 5432

## Offensive Angle
Attackers scan ports to discover services.

Common scan examples:

```bash
nmap -sV --top-ports 1000 target.com
nmap -sS -p- target.com
nmap -sV -sC target.com
nmap -sU --top-ports 100 target.com
```

What attackers look for:

- Open remote access.
- Outdated services.
- Exposed databases.
- SMB on Windows hosts.
- Default credentials.

## SOC Detection
Watch for:

- One IP hitting many ports.
- One IP hitting many hosts.
- Repeated RDP failures.
- Port 445 spikes.
- Telnet traffic.
- Database port exposed to internet.

## Interview Ready Answers
**Why is Telnet insecure?**

Telnet sends traffic in clear text, including passwords.
Anyone capturing traffic can read the session.

**What is the difference between port 80 and 443?**

Port 80 is HTTP and unencrypted.
Port 443 is HTTPS and encrypted using TLS.

**Why is port 445 important in security?**

Port 445 is SMB.
It is commonly used in Windows networks and has been targeted by worms, ransomware, and lateral movement attacks.

## Quick Revision
- SSH = 22.
- DNS = 53.
- HTTP = 80.
- HTTPS = 443.
- SMB = 445.
- RDP = 3389.
- Ports tell you what service is running.
