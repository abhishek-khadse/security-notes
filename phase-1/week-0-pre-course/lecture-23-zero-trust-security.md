# Lecture 23: Zero Trust Security
Date: May 20, 2026
Source: NetworkChuck Free CCNA

## Core Principle
"Never Trust, Always Verify"
No user or device trusted automatically
— even inside the network.

## Why Zero Trust?
Traditional model assumed internal = safe.
Modern reality:
- Remote work = no clear perimeter
- Cloud = data everywhere
- Insider threats = internal attackers
- Ransomware = spreads laterally once inside

## Three Pillars
| Pillar | Meaning |
|--------|---------|
| Verify Explicitly | Always authenticate + authorize |
| Least Privilege | Minimum access needed, nothing more |
| Assume Breach | Act like attacker is already inside |

## Zero Trust Architecture
```
User/Device
        ↓
Identity Verification (IAM + MFA)
        ↓
Device Health Check (EDR)
        ↓
Policy Engine (allow/deny)
        ↓
Specific Resource Access only
        ↓
Continuous Monitoring (SIEM)
```

## Key Components
| Component | Purpose | Examples |
|-----------|---------|----------|
| IAM | Identity management | Azure AD, Okta |
| MFA | Multi-factor auth | Password + OTP |
| Microsegmentation | Network isolation | VLANs, SDN |
| EDR | Endpoint security | CrowdStrike, SentinelOne |
| SIEM | Continuous monitoring | QRadar, Sentinel |
| ZTNA | Replace VPN | Zscaler, Cloudflare |

## VPN vs ZTNA
| Feature | VPN | ZTNA |
|---------|-----|------|
| Access | Full network | Specific app only |
| Trust | After login = trusted | Continuous verification |
| Attack surface | Large | Minimal |
| Lateral movement | Easy | Very difficult |

## Offensive Angle
Zero Trust makes attacker's life harder —
but here's how attackers still bypass it:

| Bypass Method | Description |
|---------------|-------------|
| Identity attacks | Steal valid credentials = bypass IAM |
| MFA fatigue | Spam MFA requests until user approves |
| Device impersonation | Compromise trusted endpoint |
| Token theft | Steal OAuth/session token post-auth |
| Insider threat | Legitimate user abuses least privilege |
| Supply chain | Compromise trusted software/vendor |

### MFA Fatigue Attack
Attacker has valid username + password
Triggers MFA notification repeatedly
User gets 50 notifications at 3 AM
User accidentally approves one
Attacker is in — Zero Trust bypassed
Real example: Uber breach 2022
Attacker used MFA fatigue attack
Gained full internal access

### Token Theft — Bypass Zero Trust
Even with MFA, once authenticated
= session token issued
Attacker targets:

Browser cookies
Memory (LSASS)
OAuth tokens

Token stolen = replay authentication
= attacker IS the verified user
= Zero Trust sees legitimate session

### Lateral Movement in Zero Trust
Traditional: compromise one host = move freely
Zero Trust: microsegmentation stops movement
BUT if attacker compromises:

Service account with broad permissions
Admin account
SIEM/monitoring system itself

= Bypass microsegmentation
= Still move laterally

## SOC Detection
| Attack | Detection |
|--------|-----------|
| MFA fatigue | Multiple MFA denials then approval |
| Token theft | Same token from 2 different IPs |
| Impossible travel | Login Mumbai then London 5 mins later |
| Privilege escalation | User accessing beyond normal scope |
| Lateral movement | Traffic between microsegments |
| Credential stuffing | Multiple failed logins across accounts |

### Sentinel/QRadar Zero Trust Rules
Rule 1: MFA approved after 10+ denials = alert
Rule 2: Session token used from new IP = alert
Rule 3: Access outside normal hours = flag
Rule 4: Cross-segment traffic = investigate
Rule 5: New device accessing sensitive data = verify
Rule 6: Admin login + immediate data export = alert

## Zero Trust in Real Attacks
### Uber Breach 2022
Attack path:

Attacker bought Uber employee credentials
MFA fatigue attack — spammed notifications
Employee approved MFA on WhatsApp
Attacker inside network
Found admin credentials in PowerShell script
Accessed internal tools, Slack, HackerOne
Full compromise

Lesson: Zero Trust failed at MFA fatigue and privileged credential exposure.

## Interview Questions
**Q: What is Zero Trust?**
Security model — never trust, always verify.
Every access request authenticated and
authorized regardless of location.
No implicit trust even inside network.

**Q: What is MFA fatigue attack?**
Attacker spams MFA push notifications.
Overwhelmed user accidentally approves.
Bypasses MFA without stealing the factor.
Defense: number matching MFA, limit attempts.

**Q: Least Privilege in practice?**
HR employee gets HR system access only.
Not finance, not servers, not admin panels.
Even if compromised = limited damage radius.

**Q: Zero Trust vs traditional perimeter?**
Traditional = castle and moat.
Inside = trusted, outside = untrusted.
Zero Trust = verify everyone everywhere.
No perimeter — identity IS the perimeter.

**Q: What is microsegmentation?**
Divide network into small isolated zones.
Breach in one zone cannot spread to others.
Limits lateral movement significantly.
