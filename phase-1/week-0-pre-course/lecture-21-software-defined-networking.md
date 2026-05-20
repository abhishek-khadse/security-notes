# Lecture 21: Software Defined Networking (SDN)
Date: May 20, 2026
Source: NetworkChuck Free CCNA

## What is SDN?
Network controlled by software instead of hardware.
Separates Control Plane from Data Plane.
Central controller manages entire network.

## Traditional vs SDN
| Feature | Traditional | SDN |
|---------|-------------|-----|
| Control | Per device | Centralized |
| Config | Manual | Automated |
| Scaling | Complex | Easy |
| Management | Device-by-device | Single controller |

## SDN Architecture
```
Application Layer   ← Your apps, security tools
        ↓ Northbound API
Control Layer       ← SDN Controller (brain)
        ↓ Southbound API (OpenFlow)
Infrastructure      ← Switches, routers (just forward)
```

## Key Components
| Component | Purpose |
|-----------|---------|
| SDN Controller | Central brain — makes all decisions |
| OpenFlow | Protocol between controller + devices |
| Northbound API | Apps talk to controller |
| Southbound API | Controller talks to devices |
| Control Plane | Makes routing decisions |
| Data Plane | Forwards actual packets |

## Popular SDN Controllers
| Controller | Type |
|------------|------|
| OpenDaylight | Open source |
| ONOS | Open source |
| Cisco APIC | Enterprise |
| VMware NSX | Cloud/DC |

## Offensive Angle
| Target | Attack |
|--------|--------|
| SDN Controller | Single point of failure — take it down = entire network down |
| Northbound API | REST API attacks, unauthorized access |
| Southbound API | OpenFlow manipulation |
| Controller auth | Default credentials on controller |
| Misconfiguration | One wrong policy = network-wide impact |

### SDN Controller Attack — High Value Target
Traditional network: compromise 1 switch
= access to 1 segment
SDN network: compromise controller
= control ENTIRE network
= reroute all traffic
= create backdoor flows
= disable security policies network-wide
SDN controller = crown jewel of modern networks

### OpenFlow Exploitation
OpenFlow has no built-in encryption by default
Attacker on network can:

Intercept controller-switch communication
Inject malicious flow rules
Redirect traffic to attacker machine
Drop traffic = DoS entire network

### REST API Attacks on Northbound
```bash
# Test for exposed SDN controller API
curl http://controller-ip:8080/restconf/
curl http://controller-ip:8181/onos/v1/

# Common default creds
# OpenDaylight: admin/admin
# ONOS: onos/rocks

# If accessible — read all flow rules
# Inject malicious flows
# Reroute traffic
```

## SOC Detection
| Attack | Detection |
|--------|-----------|
| Controller attack | Unusual controller API calls |
| Flow rule injection | Unexpected flow rules added |
| Controller DoS | Network-wide connectivity loss |
| API brute force | Failed auth on controller API |
| OpenFlow tampering | Unsigned flow modifications |

### QRadar/Sentinel Rules for SDN
Rule 1: Controller API access from unknown IP
Rule 2: Mass flow rule changes in short time
Rule 3: Controller unreachable = critical alert
Rule 4: OpenFlow connection from unexpected source
Rule 5: Traffic rerouted to unknown destination

## SDN vs NFV
| SDN | NFV |
|-----|-----|
| Controls network paths | Virtualizes network functions |
| Where traffic goes | What processes the traffic |
| Controller-based | VM-based |
| Example: Cisco ACI | Example: Virtual firewall |

## Interview Questions
**Q: What is the biggest security risk in SDN?**
Single point of failure — the controller.
Compromising controller = controlling entire network.
Traditional networks require compromising
each device individually.

**Q: Control Plane vs Data Plane?**
Control Plane = brain, makes routing decisions.
Data Plane = muscles, forwards actual packets.
SDN separates these — controller handles
control, switches only handle forwarding.

**Q: What is OpenFlow?**
Protocol for communication between
SDN controller and network devices.
Controller tells switches exactly how
to forward each type of traffic.

**Q: Why is SDN important for security?**
Centralized policies = consistent enforcement.
Dynamic response = faster threat containment.
Traffic visibility = better monitoring.
BUT controller compromise = catastrophic.
