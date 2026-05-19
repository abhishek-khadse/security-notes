# Lecture 05: Designing the Cloud
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## What is Cloud Computing?
Cloud computing means delivering compute, storage, databases, networking, and software over the internet.
Instead of buying physical infrastructure, users rent resources from a cloud provider.

## Cloud Service Models
| Model | Full Form | User Controls | Examples |
|-------|-----------|---------------|----------|
| IaaS | Infrastructure as a Service | OS, apps, data | AWS EC2, Azure VM |
| PaaS | Platform as a Service | Apps, data | Heroku, Google App Engine |
| SaaS | Software as a Service | Usage and configuration | Gmail, Microsoft 365 |

## Memory Trick
- IaaS = rent infrastructure.
- PaaS = rent a platform.
- SaaS = use ready software.

## Basic Cloud Architecture
User -> DNS -> Load Balancer -> Web Server -> Application Server -> Database

## Scaling Types
| Type | Method | When to Use |
|------|--------|-------------|
| Vertical | Add CPU/RAM to one server | Quick fix or small workloads |
| Horizontal | Add more servers | Production and high availability |

## Cloud Security Basics
| Control | Purpose |
|---------|---------|
| IAM | Identity and access management |
| Security Groups | Instance-level firewall rules |
| Network ACLs | Subnet-level traffic filtering |
| Encryption | Protect data at rest and in transit |
| MFA | Add extra login protection |
| Logging | Track activity and support investigations |

## Offensive Angle
| Component | Example Attack |
|-----------|----------------|
| IAM | Privilege escalation, stolen access keys |
| Storage Buckets | Public exposure through misconfiguration |
| Metadata Service | SSRF to steal cloud credentials |
| Load Balancer | DDoS targeting |
| Logs | Disabled or deleted logs to hide activity |

### Famous Cloud Attack - SSRF to Metadata
1. Attacker finds an SSRF vulnerability.
2. Attacker requests the cloud metadata service.
3. Example: `http://169.254.169.254/latest/meta-data/`
4. Attacker obtains temporary IAM credentials.
5. Stolen credentials may lead to cloud account compromise.

## SOC Detection
| Attack | Detection |
|--------|-----------|
| IAM abuse | Unusual API calls in CloudTrail or activity logs |
| Data exfiltration | Large storage download alert |
| Metadata access | SSRF pattern in WAF or application logs |
| Crypto mining | Unusual compute spike |
| Log tampering | Logging disabled or deleted trail alerts |

## Interview Questions
**Q: What is the difference between IaaS, PaaS, and SaaS?**
IaaS gives the most control because you manage the OS and apps.
PaaS lets you manage the application and data while the provider manages the platform.
SaaS is ready-to-use software managed mostly by the provider.

**Q: What is horizontal scaling?**
Horizontal scaling means adding more servers or instances to handle load.
It is commonly preferred in cloud environments because it supports redundancy and high availability.
