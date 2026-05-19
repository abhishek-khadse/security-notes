# Lecture 04: Networking Functions
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## Big Idea
A network is not just cables and IP addresses.
The purpose of a network is to let people, devices, and applications communicate safely and reliably.

When studying any network, ask:

- What needs to communicate?
- What resources are being shared?
- Who is allowed to access what?
- How is traffic protected?
- How is activity monitored?

## Communication
The most basic function of a network is communication.

Examples:

- Sending email.
- Opening a website.
- Making a VoIP call.
- Sending chat messages.

Security angle:

Attackers may abuse normal communication channels for phishing, command-and-control, or data exfiltration.

## Resource Sharing
Networks allow users to share resources.

Examples:

- Shared folders.
- Printers.
- Databases.
- Internal applications.

Security angle:

Shared resources are common attack targets.
If permissions are weak, attackers can steal files or move laterally.

## Data Transfer
Networks move data between systems.

Examples:

- Downloading files.
- Uploading backups.
- Syncing cloud storage.
- Transferring logs to a SIEM.

Security angle:

Large or unusual data transfers can indicate exfiltration.

## Remote Access
Remote access lets users connect to systems from another location.

Common tools:

- SSH
- RDP
- VPN

Security angle:

Remote access is heavily attacked because it can give direct access to internal systems.

Common attacks:

- SSH brute force.
- RDP brute force.
- Stolen VPN credentials.
- MFA fatigue attacks.

Example commands:

```bash
hydra -l admin -P wordlist.txt ssh://192.168.1.1
hydra -l administrator -P wordlist.txt rdp://192.168.1.1
```

## Centralized Management
Large networks need centralized management.

Example:

Active Directory manages users, computers, groups, authentication, and policies in many Windows environments.

Security angle:

If Active Directory is compromised, the attacker may control the whole organization.

Watch for:

- New admin accounts.
- Unusual group changes.
- Suspicious Kerberos activity.
- Login activity from unexpected systems.

## Security
Networks need controls to protect traffic and systems.

Examples:

- Firewalls.
- IDS/IPS.
- TLS.
- IPsec.
- MFA.
- Network segmentation.

Security is not one device.
It is a combination of prevention, detection, and response.

## Scalability and Reliability
A good network should grow and stay available.

Scalability means the network can handle more users, systems, or traffic.
Reliability means the network keeps working even when something fails.

Examples:

- Redundant links.
- Load balancers.
- Backup internet connections.
- Multiple servers.

## SOC Analyst View
Important detections:

- Multiple failed SSH or RDP logins.
- New unknown device on the network.
- SMB traffic spikes.
- Unusual DNS traffic.
- Large outbound file transfers.
- Privilege changes in Active Directory.

## Interview Ready Answers
**What is Active Directory?**

Active Directory is Microsoft's centralized identity management system.
It manages users, computers, authentication, groups, and policies in Windows networks.

**Why is SSH better than Telnet?**

SSH encrypts traffic.
Telnet sends everything in clear text, including passwords.

## Quick Revision
- Network = communication + sharing + access + security.
- Remote access is useful but risky.
- Active Directory is a major target.
- Logs are as important as devices.
- Security means prevent, detect, and respond.
