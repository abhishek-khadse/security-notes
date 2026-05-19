# Lecture 07: Introduction to IP
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## Big Idea
IP stands for Internet Protocol.
It is used to identify devices and move packets between networks.

Simple meaning:

IP is like the address system of networking.
If data needs to travel from one network to another, IP helps decide where it should go.

## IP Address
An IP address is a logical address.
It can change depending on the network.

Example:

```text
192.168.1.10
```

This address tells the network where a device is located logically.

## MAC Address vs IP Address
MAC address is used inside the local network.
IP address is used to route traffic between networks.

Easy memory:

- MAC = local delivery.
- IP = network-to-network delivery.

Example:

If you send a package:

- IP address is like the city and street address.
- MAC address is like the exact person or door inside the local building.

## IPv4
IPv4 uses 32-bit addresses.

Example:

```text
192.168.1.1
```

IPv4 has about 4.3 billion addresses.
Because that is not enough for the whole world, private IP ranges and NAT are widely used.

## IPv6
IPv6 uses 128-bit addresses.

Example:

```text
2001:db8::1
```

IPv6 has a very large address space.
IPv6 supports IPsec, but encryption is not automatic just because IPv6 is used.

## Private IP Ranges
Private IPs are used inside internal networks.
They are not directly routed on the public internet.

Important ranges:

- `10.0.0.0/8`
- `172.16.0.0/12`
- `192.168.0.0/16`

Common home networks usually use:

```text
192.168.1.0/24
```

## Subnet Mask
A subnet mask separates the network part from the host part of an IP address.

Example:

```text
192.168.1.10/24
```

Here, `/24` means the first 24 bits are the network portion.

In a `/24` network:

- Network: `192.168.1.0`
- Usable hosts: `192.168.1.1` to `192.168.1.254`
- Broadcast: `192.168.1.255`

## Default Gateway
The default gateway is the router used to reach other networks.

Simple meaning:

If your computer does not know where to send traffic, it sends it to the default gateway.

## DNS
DNS converts names into IP addresses.

Example:

```text
google.com -> IP address
```

Without DNS, users would need to remember IP addresses instead of names.

## DHCP
DHCP automatically gives network settings to devices.

It can provide:

- IP address.
- Subnet mask.
- Default gateway.
- DNS server.

## Offensive Angle
Attackers use IP-related protocols for discovery and attacks.

Examples:

- Ping sweep to find live hosts.
- ARP spoofing for man-in-the-middle attacks.
- Rogue DHCP server to give wrong gateway or DNS.
- DNS poisoning to redirect users.
- ICMP flood for denial of service.

Useful commands:

```bash
nmap -sn 192.168.1.0/24
nslookup target.com
dig target.com
arp -a
traceroute target.com
```

## SOC Detection
Watch for:

- Rogue DHCP server.
- High ICMP volume.
- DNS response mismatch.
- Same IP using different MAC addresses.
- Suspicious DNS tunneling behavior.

## Interview Ready Answers
**What is the difference between MAC and IP address?**

MAC is a Layer 2 physical address used for local delivery.
IP is a Layer 3 logical address used for routing between networks.

**What is DHCP?**

DHCP automatically assigns IP address, subnet mask, default gateway, and DNS server information to devices.

**What is a default gateway?**

The default gateway is the router that forwards traffic outside the local network.

## Quick Revision
- IP = logical address.
- MAC = physical/local address.
- DNS = name to IP.
- DHCP = automatic IP settings.
- Default gateway = router to other networks.
