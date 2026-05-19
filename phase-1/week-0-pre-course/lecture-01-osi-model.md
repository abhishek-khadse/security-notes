# Lecture 01: OSI Model
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## Big Idea
The OSI model is a way to understand how data moves from one device to another.
It splits network communication into 7 layers, so troubleshooting becomes easier.

Think of it like this:

1. A user opens an app.
2. The app creates data.
3. The data is prepared, split, addressed, framed, and finally sent as signals.
4. The receiving device reverses the process.

For cybersecurity, this is useful because every attack and every detection happens somewhere in this flow.

## The 7 Layers in Simple Words
### Layer 7 - Application
This is closest to the user.
It includes services like websites, email, DNS, file transfer, and login portals.

Examples:
- HTTP and HTTPS
- DNS
- FTP
- SMTP

Security examples:
- Phishing
- SQL injection
- XSS
- DNS abuse

### Layer 6 - Presentation
This layer is about data format, encoding, compression, and encryption.

Simple meaning:
It makes sure data is in a format the application can understand.

Examples:
- TLS encryption
- JPEG, PNG, MP3
- Encoding and decoding

Security examples:
- TLS downgrade
- Encoding bypass
- Weak encryption

### Layer 5 - Session
This layer manages sessions between systems.

Simple meaning:
It starts, maintains, and ends conversations between applications.

Security examples:
- Session hijacking
- Stolen cookies
- Abnormal login sessions

### Layer 4 - Transport
This layer controls communication between hosts using ports.

Main protocols:
- TCP
- UDP

Simple meaning:
IP tells us which device.
Port tells us which service on that device.

Security examples:
- Port scanning
- SYN flood
- UDP flood

### Layer 3 - Network
This layer handles IP addresses and routing.

Simple meaning:
It decides where packets should go across networks.

Examples:
- IP
- ICMP
- Routers

Security examples:
- IP spoofing
- ICMP scanning
- Routing attacks

### Layer 2 - Data Link
This layer works inside the local network using MAC addresses.

Examples:
- Ethernet
- ARP
- Switches

Security examples:
- ARP spoofing
- MAC flooding
- VLAN hopping

### Layer 1 - Physical
This is the actual physical signal.

Examples:
- Copper cable
- Fiber cable
- Wi-Fi radio signal
- Hubs

Security examples:
- Cable tapping
- Cable cut
- Rogue device plugged into a port

## Easy Memory Trick
From top to bottom:

**All People Seem To Need Data Processing**

- Application
- Presentation
- Session
- Transport
- Network
- Data Link
- Physical

## How to Think Like a SOC Analyst
When an alert comes in, ask:

- Is this a web/app issue? Layer 7.
- Is this a TLS or encoding issue? Layer 6.
- Is this a login/session issue? Layer 5.
- Is this a port or TCP/UDP issue? Layer 4.
- Is this an IP/routing issue? Layer 3.
- Is this a MAC/ARP/switching issue? Layer 2.
- Is this a cable/device/link issue? Layer 1.

## Interview Ready Answer
**What is the OSI model?**

The OSI model is a 7-layer framework used to understand and standardize network communication.
Each layer has a specific role, from user-facing applications at Layer 7 down to physical signals at Layer 1.
It is also useful for troubleshooting and security analysis.

## Quick Revision
- Switch = Layer 2 = MAC address.
- Router = Layer 3 = IP address.
- Firewall can work across multiple layers depending on type.
- WAF protects Layer 7 web applications.
- Physical attacks are Layer 1.
