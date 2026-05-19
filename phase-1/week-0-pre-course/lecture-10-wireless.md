# Lecture 10: Wireless Networking
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## Big Idea
Wireless networking sends data through radio waves instead of cables.

Wi-Fi is convenient, but security is harder because signals travel through the air.
An attacker may not need physical access to a cable.
They may only need to be near the wireless signal.

## Types of Wireless Networks
### WPAN
Wireless Personal Area Network.

Example:

- Bluetooth headphones.
- Smartwatch.

### WLAN
Wireless Local Area Network.

Example:

- Home Wi-Fi.
- Office Wi-Fi.

### WMAN
Wireless Metropolitan Area Network.

Example:

- City-wide wireless network.
- WiMAX.

### WWAN
Wireless Wide Area Network.

Example:

- 4G.
- 5G.

## Wi-Fi Standards
The names like `802.11n`, `802.11ac`, and `802.11ax` are Wi-Fi standards.

Beginner memory:

- 802.11b and 802.11g are old.
- 802.11n supports 2.4 GHz and 5 GHz.
- 802.11ac is mainly 5 GHz and faster.
- 802.11ax is Wi-Fi 6 and more modern.

## 2.4 GHz vs 5 GHz
2.4 GHz:

- Longer range.
- Better wall penetration.
- More interference.
- Slower.

5 GHz:

- Shorter range.
- Weaker wall penetration.
- Less interference.
- Faster.

Easy memory:

2.4 GHz goes farther.
5 GHz goes faster.

## Wireless Security Protocols
### WEP
WEP is broken.
Do not use it.

### WPA
WPA is old.
Better than WEP, but not recommended today.

### WPA2
WPA2 is still very common.
It is strong when configured with a strong password.

### WPA3
WPA3 is the modern option.
It improves protection against offline password guessing.

## Common Wireless Attacks
### Evil Twin
The attacker creates a fake access point with the same or similar name as the real Wi-Fi.

Goal:

- Trick users into connecting.
- Capture credentials.
- Intercept traffic.

### Deauthentication Attack
The attacker sends deauth frames to disconnect users from Wi-Fi.

Goal:

- Force users to reconnect.
- Capture WPA handshake.
- Disrupt wireless service.

### WPA Handshake Capture
The attacker captures the handshake when a user connects.
Then they try to crack the password offline using a wordlist.

Example process:

```bash
airmon-ng start wlan0
airodump-ng -c 6 --bssid XX:XX:XX:XX:XX:XX wlan0mon
aireplay-ng -0 5 -a XX:XX:XX:XX:XX:XX wlan0mon
aircrack-ng capture.cap -w /usr/share/wordlists/rockyou.txt
```

### Rogue AP
A rogue access point is an unauthorized Wi-Fi device connected to the network.

Risk:

It may bypass normal security controls.

## SOC Detection
Watch for:

- Duplicate SSID.
- Unknown BSSID.
- Unknown wireless MAC.
- Many deauth frames.
- Users repeatedly disconnecting.
- New access point near office.

Note:

Packet sniffing is hard to detect directly.
Wireless IDS/WIPS tools help detect suspicious wireless behavior.

## Interview Ready Answers
**What is an Evil Twin attack?**

An Evil Twin is a fake Wi-Fi access point that looks like a real network.
Users may connect to it and expose credentials or traffic.

**Why is WEP insecure?**

WEP uses weak cryptography and can be cracked quickly using common tools.

**What is the difference between WPA2 and WPA3?**

WPA3 uses SAE and improves resistance to offline dictionary attacks.
WPA2 is still common, but password strength matters a lot.

## Quick Revision
- 2.4 GHz = longer range.
- 5 GHz = faster speed.
- WEP = broken.
- WPA2 = common.
- WPA3 = modern.
- Evil Twin = fake Wi-Fi.
- Deauth = disconnect users.
