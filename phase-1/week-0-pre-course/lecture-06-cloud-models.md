# Lecture 06: Cloud Models
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## Cloud Deployment Models
| Model | Ownership | Security Control | Cost | Use Case |
|-------|-----------|------------------|------|----------|
| Public | Cloud provider | Shared | Lower | Startups, web apps |
| Private | Organization | High | Higher | Banks, government |
| Hybrid | Provider + organization | Mixed | Medium | Enterprise workloads |
| Community | Shared group | Shared | Medium | Healthcare, research |

## Shared Responsibility Model
The provider and customer both have security responsibilities.
The exact split depends on whether the service is IaaS, PaaS, or SaaS.

| Responsibility | Provider | Customer |
|----------------|----------|----------|
| Physical security | Yes | No |
| Hardware | Yes | No |
| Hypervisor / platform | Usually | Sometimes |
| Data security | No | Yes |
| User access | No | Yes |
| Application security | Depends on model | Depends on model |
| Configuration | Depends on model | Depends on model |

## Offensive Angle
| Cloud Model | Attack Surface |
|-------------|----------------|
| Public Cloud | Misconfigured storage buckets, exposed keys |
| Private Cloud | Internal network attacks |
| Hybrid Cloud | Weak connection between environments |
| SaaS | Account takeover, OAuth abuse |
| PaaS | Insecure application and API endpoints |
| IaaS | Exposed management ports and weak VM hardening |

### Common Cloud Misconfigurations Attackers Look For
```bash
# Find exposed S3 buckets
# Tools: S3Scanner, CloudBrute

# Check for exposed metadata service from a cloud host
curl http://169.254.169.254/latest/meta-data/

# Find exposed cloud storage or cloud-related issues
subfinder -d target.com | httpx | nuclei -t cloud
```

## SOC Detection
| Threat | Detection Method |
|--------|------------------|
| Account takeover | Unusual login location or impossible travel |
| Data exfiltration | Large download from storage |
| Privilege escalation | New admin role or policy assignment |
| Crypto mining | CPU spike and unusual instance creation |
| Exposed keys | API calls from unknown IPs |

## Interview Questions
**Q: What is the Shared Responsibility Model?**
The cloud provider secures the underlying infrastructure.
The customer secures their data, users, access, applications, and configurations depending on the service model.

**Q: Why do enterprises use hybrid cloud?**
They can keep sensitive data or legacy systems in a private environment while using public cloud for scalable or cheaper workloads.
