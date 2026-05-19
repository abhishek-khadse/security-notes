# Lecture 06: Cloud Models
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## Big Idea
Cloud models explain where the cloud runs, who owns it, and who is responsible for security.

There are two things to remember:

1. Deployment model = where the cloud is hosted.
2. Shared responsibility = what the provider secures vs what the customer secures.

## Public Cloud
Public cloud is owned and operated by a cloud provider.

Examples:

- AWS
- Azure
- Google Cloud

Simple meaning:

You rent resources from a provider's infrastructure.

Good for:

- Startups.
- Web applications.
- Fast scaling.
- Lower upfront cost.

Security concern:

The provider secures the infrastructure, but you must configure your resources correctly.
A public S3 bucket or exposed VM is usually the customer's mistake.

## Private Cloud
Private cloud is used by one organization.
It may run in the organization's data center or be hosted by a provider.

Good for:

- Banks.
- Government.
- Sensitive workloads.
- Strict compliance needs.

Security concern:

The organization has more control, but also more responsibility.
Internal attackers and misconfigurations still matter.

## Hybrid Cloud
Hybrid cloud combines private and public cloud.

Example:

A company keeps sensitive databases in a private environment but runs web applications in public cloud.

Good for:

- Enterprises.
- Migration projects.
- Compliance-heavy workloads.
- Cost optimization.

Security concern:

The connection between environments becomes very important.
Weak VPNs, poor routing, or bad identity integration can create risk.

## Community Cloud
Community cloud is shared by organizations with similar needs.

Examples:

- Healthcare groups.
- Research organizations.
- Government agencies.

Security concern:

All members must follow common security and compliance rules.

## Shared Responsibility Model
This is one of the most important cloud concepts.

Simple meaning:

The cloud provider secures the cloud.
The customer secures what they put in the cloud.

Provider usually handles:

- Data center security.
- Physical servers.
- Storage hardware.
- Network hardware.
- Core infrastructure.

Customer usually handles:

- Data.
- Users.
- Passwords and MFA.
- IAM permissions.
- Application security.
- Cloud configuration.
- Operating system patching in IaaS.

## Easy Example
If an AWS data center has a physical security issue, that is AWS responsibility.

If you make a storage bucket public and leak data, that is your responsibility.

## Offensive Angle
Attackers look for mistakes in customer-controlled areas.

Common targets:

- Public storage buckets.
- Exposed RDP or SSH.
- Stolen cloud keys.
- Weak IAM policies.
- Over-permissive roles.
- Unsecured APIs.
- Weak hybrid cloud connections.

Example commands:

```bash
# Check metadata service from a cloud host
curl http://169.254.169.254/latest/meta-data/

# Find subdomains and test web services
subfinder -d target.com | httpx
```

## SOC Detection
Watch for:

- Login from unusual location.
- New admin role assignment.
- Large storage downloads.
- API calls from unknown IPs.
- New cloud instances created suddenly.
- High CPU usage that may indicate crypto mining.
- Logging disabled or audit trail deleted.

## Interview Ready Answers
**What is the shared responsibility model?**

The shared responsibility model defines what the cloud provider secures and what the customer secures.
The provider usually secures the infrastructure.
The customer secures data, access, applications, and configurations.

**Why do enterprises use hybrid cloud?**

Hybrid cloud lets enterprises keep sensitive systems private while using public cloud for scale, flexibility, or lower cost.

## Quick Revision
- Public cloud = provider-owned shared infrastructure.
- Private cloud = used by one organization.
- Hybrid cloud = private + public together.
- Provider secures the cloud.
- Customer secures what they configure and deploy.
