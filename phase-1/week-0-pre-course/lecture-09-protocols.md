# Lecture 09: Networking Protocols
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## Big Idea
A protocol is a set of rules.
Networking protocols define how devices communicate.

Instead of memorizing a big table, learn protocols by their job.

Ask:

- Does it find addresses?
- Does it give IP settings?
- Does it move web traffic?
- Does it transfer files?
- Does it manage devices?
- Does it support remote access?

## Address and Discovery Protocols
### ARP
ARP maps an IP address to a MAC address inside a local network.

Simple example:

Your computer knows the gateway IP, but it needs the gateway MAC address.
ARP asks, "Who has this IP?"
The gateway replies with its MAC address.

Security problem:

ARP has no built-in authentication.
Attackers can send fake ARP replies and perform man-in-the-middle attacks.

### ICMP
ICMP is used for network diagnostics.

Common example:

```bash
ping 8.8.8.8
```

Security problem:

Attackers use ICMP for discovery and flooding.

## IP Configuration Protocol
### DHCP
DHCP automatically gives IP settings to devices.

It provides:

- IP address.
- Subnet mask.
- Default gateway.
- DNS server.

Security problem:

A rogue DHCP server can give victims a fake gateway or DNS server.

## Name Resolution Protocol
### DNS
DNS converts domain names into IP addresses.

Example:

```text
google.com -> IP address
```

Security problems:

- DNS poisoning.
- DNS tunneling.
- Suspicious domain lookups.
- Malware command-and-control.

## Web Protocols
### HTTP
HTTP is normal web traffic on port 80.

Problem:

It is not encrypted.

### HTTPS
HTTPS is encrypted web traffic on port 443.

Important:

HTTPS protects data in transit, but attackers can still host malicious websites over HTTPS.

## Remote Access Protocols
### SSH
SSH runs on port 22.
It is used for secure remote login.

Security issue:

Brute force and stolen keys.

### Telnet
Telnet runs on port 23.
It is old and insecure.

Security issue:

Everything is sent in clear text.

### RDP
RDP runs on port 3389.
It is used for Windows remote desktop.

Security issue:

Common target for brute force and ransomware access.

## File and Email Protocols
### FTP
FTP uses ports 20 and 21.
It transfers files but can expose credentials in clear text.

### SFTP
SFTP uses SSH, usually port 22.
It is safer than FTP.

### SMTP
SMTP sends email, commonly on port 25.
It can be abused for spoofing or relay attacks.

### POP3 and IMAP
Both are used to receive email.

Easy difference:

- POP3 downloads mail.
- IMAP syncs mail across devices.

## Management and Directory Protocols
### SNMP
SNMP is used to manage network devices.

Security issue:

Default community strings like `public` can leak device information.

### LDAP
LDAP is used for directory services like Active Directory queries.

Security issue:

Can be abused for enumeration if exposed or misconfigured.

### SMB
SMB is used for Windows file sharing.

Security issue:

Major target for ransomware and lateral movement.

### NTP
NTP synchronizes time.

Security issue:

Can be abused in amplification attacks.

## Offensive Angle
Example commands:

```bash
# DNS enumeration
dig any target.com
dnsenum target.com

# SMTP enumeration
nmap -p 25 --script smtp-enum-users target.com

# SNMP enumeration
snmpwalk -c public -v1 192.168.1.1
```

## SOC Detection
Watch for:

- ARP spoofing alerts.
- High DNS volume.
- Long strange DNS subdomains.
- SMB spikes.
- RDP brute force.
- SNMP using default community strings.
- NTP flood behavior.

## Interview Ready Answers
**What is DNS tunneling?**

DNS tunneling hides data inside DNS queries.
Attackers may encode data into subdomains to bypass security controls.

**What is the difference between POP3 and IMAP?**

POP3 downloads emails to a device.
IMAP syncs emails across devices.

## Quick Revision
- ARP = IP to MAC.
- DHCP = gives IP settings.
- DNS = name to IP.
- SSH = secure remote login.
- Telnet = insecure remote login.
- SMB = Windows file sharing.
- SNMP = network device management.
