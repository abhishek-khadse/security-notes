# Lecture 02: OSI Layers 3, 4, and 5
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## Why These Layers Matter
Layers 3, 4, and 5 are very important for networking and security.

These layers answer three big questions:

- Layer 3: Which device or network should the packet go to?
- Layer 4: Which service or port should receive it?
- Layer 5: Which session or conversation does it belong to?

## Layer 3 - Network Layer
Layer 3 is the routing layer.
It uses IP addresses to move packets between networks.

Simple example:

Your laptop wants to visit `google.com`.
Your laptop cannot directly reach Google, so it sends traffic to the default gateway.
The router then forwards the packet toward the destination network.

Important Layer 3 ideas:

- IP addressing
- Routing
- ICMP
- Default gateway
- Packet forwarding

Common devices:

- Router
- Layer 3 switch

Common protocols:

- IP
- ICMP
- IPsec

### Security View
Attackers use Layer 3 for discovery and traffic manipulation.

Examples:

- Ping sweep to find live hosts.
- Traceroute to map network paths.
- IP spoofing to fake the source address.
- ICMP flood to overload a target.

Useful commands:

```bash
nmap -sn 192.168.1.0/24
traceroute target.com
```

### SOC Detection
Watch for:

- Too much ICMP traffic.
- Scans across many IP addresses.
- Traffic from suspicious or impossible source IPs.
- Communication with known malicious IPs.

## Layer 4 - Transport Layer
Layer 4 is about ports and transport protocols.

Simple meaning:

Layer 3 gets traffic to the correct machine.
Layer 4 gets traffic to the correct service on that machine.

Example:

- `192.168.1.10:22` means SSH on that host.
- `192.168.1.10:443` means HTTPS on that host.

## TCP vs UDP
TCP is reliable.
It creates a connection and checks that data arrives properly.

UDP is faster.
It sends data without building a full connection first.

Use TCP when reliability matters:

- HTTPS
- SSH
- RDP

Use UDP when speed matters:

- DNS
- VoIP
- Online gaming

## TCP 3-Way Handshake
TCP starts with a 3-step process:

1. Client sends SYN.
2. Server replies SYN-ACK.
3. Client sends ACK.

After this, the connection is established.

### SYN Flood
A SYN flood abuses this handshake.

The attacker sends many SYN packets but never completes the handshake.
The server keeps waiting, resources fill up, and real users may be blocked.

### SOC Detection
Watch for:

- Many SYN packets.
- Many half-open connections.
- Sequential port hits.
- One source scanning many ports.

Useful commands:

```bash
nmap -sS -p- target.com
nmap -sU target.com
```

## Layer 5 - Session Layer
Layer 5 manages sessions.

Simple meaning:

It keeps track of conversations between applications.
In real systems, session behavior often overlaps with authentication and application logic.

Examples:

- Login sessions.
- RPC sessions.
- NetBIOS sessions.
- VPN sessions.

### Security View
Attackers care about sessions because sessions prove that a user is already authenticated.

Examples:

- Stealing cookies.
- Reusing session tokens.
- Hijacking an active login.
- Abusing Kerberos tickets in Active Directory.

### SOC Detection
Watch for:

- Same user logged in from two far locations.
- Session token used from a new device.
- Multiple failures followed by success.
- Login activity outside normal time.

## Interview Ready Answers
**What is the TCP 3-way handshake?**

TCP starts a connection using SYN, SYN-ACK, and ACK.
This confirms both sides are ready to communicate.

**What is the difference between TCP and UDP?**

TCP is reliable and connection-oriented.
UDP is faster and connectionless, but delivery is not guaranteed.

## Quick Revision
- Layer 3 = IP address and routing.
- Layer 4 = TCP/UDP and ports.
- Layer 5 = sessions and conversations.
- IP finds the device.
- Port finds the service.
- Session tracks the conversation.
