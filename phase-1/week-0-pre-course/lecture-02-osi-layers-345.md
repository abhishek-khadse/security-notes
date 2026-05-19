# Lecture 02: OSI Layers 3, 4, 5 Deep Dive
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## Layer 3: Network Layer
- Handles logical addressing and routing.
- Uses IP addresses to move packets between networks.
- Routers and Layer 3 switches operate here.
- Fragmentation can happen at this layer when packets are too large for the path MTU.

### Devices
- Router
- Layer 3 switch

### Protocols
- IP
- ICMP
- IPsec

### Offensive Angle
- IP spoofing - faking the source IP address.
- ICMP reconnaissance - ping sweeps to discover live hosts.
- Traceroute - mapping the path between networks.

```bash
nmap -sn 192.168.1.0/24  # Ping sweep
traceroute target.com     # Map network hops
```

### SOC Detection
- ICMP flood alerts.
- Traffic from known malicious IPs.
- Unusual source IPs or impossible routing patterns.
- Repeated scanning across many hosts.

---

## Layer 4: Transport Layer
- Provides end-to-end communication between hosts.
- Uses port numbers to identify services.
- Main protocols are TCP and UDP.

### TCP vs UDP
| Feature | TCP | UDP |
|---------|-----|-----|
| Reliability | Reliable | Best effort |
| Connection | Connection-oriented | Connectionless |
| Speed | Slower | Faster |
| Examples | HTTPS, SSH, RDP | DNS, VoIP, gaming |

### Offensive Angle
- Port scanning - discovering exposed services.
- SYN flood - exhausting server connection resources.
- UDP scanning - finding services that do not use TCP.

```bash
nmap -sS -p- target.com   # TCP SYN scan
nmap -sU target.com       # UDP scan
```

### SYN Flood Summary
1. Attacker sends many SYN packets.
2. Server replies with SYN-ACK.
3. Attacker never completes the handshake.
4. Server resources fill up and legitimate clients may be blocked.

### SOC Detection
- High SYN rate from one or many sources.
- Sequential port hits.
- Many half-open TCP connections.
- Firewall, IDS/IPS, and load balancer rate-limit logs.

---

## Layer 5: Session Layer
- Manages sessions between applications.
- Handles session start, maintenance, and termination.
- In real networks, session behavior often overlaps with application and authentication systems.

### Examples
- NetBIOS
- RPC
- PPTP
- Application login sessions

### Offensive Angle
- Session hijacking - stealing or reusing a session token.
- Cookie theft - using a valid web session cookie.
- Authentication abuse - reusing stolen credentials or tokens.
- Kerberos abuse - forged tickets in Active Directory environments.

### SOC Detection
- Impossible travel alerts.
- Same user active from multiple unusual IPs.
- Multiple authentication failures followed by success.
- Session token reuse from new geography or device.

## Interview Questions
**Q: Explain the TCP 3-way handshake.**
1. Client sends SYN.
2. Server responds with SYN-ACK.
3. Client sends ACK.

After this, the connection is established. A SYN flood abuses this process by starting many connections and never finishing them.

**Q: What is the difference between TCP and UDP?**
TCP is reliable, connection-oriented, and slower.
UDP is faster, connectionless, and does not guarantee delivery.
