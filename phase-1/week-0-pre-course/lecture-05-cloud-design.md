# Lecture 05: Designing the Cloud
Date: May 19, 2026
Source: NetworkChuck Free CCNA

## Big Idea
Cloud computing means renting computing resources over the internet instead of buying and maintaining everything yourself.

Cloud can provide:

- Servers.
- Storage.
- Databases.
- Networking.
- Security services.
- Monitoring.
- Software.

The main benefit is flexibility.
You can create resources quickly, scale them, and pay based on usage.

## Simple Cloud Architecture
Imagine a user opening a website hosted in the cloud.

Flow:

1. User enters a domain name.
2. DNS resolves the domain.
3. Traffic reaches a load balancer.
4. The load balancer sends traffic to a web server.
5. The web server talks to an application server.
6. The application server reads or writes data in a database.

Short version:

User -> DNS -> Load Balancer -> Web Server -> App Server -> Database

## Cloud Service Models
### IaaS - Infrastructure as a Service
You rent infrastructure.

You manage:

- Operating system.
- Applications.
- Data.
- Security configuration.

Provider manages:

- Physical servers.
- Storage hardware.
- Networking hardware.
- Data center.

Example:

- AWS EC2.
- Azure VM.

### PaaS - Platform as a Service
You rent a platform to run your application.

You manage:

- Application code.
- Application data.

Provider manages:

- Operating system.
- Runtime.
- Scaling platform.
- Most infrastructure.

Example:

- Heroku.
- Google App Engine.

### SaaS - Software as a Service
You use ready-made software.

You mainly manage:

- Users.
- Settings.
- Data.
- Access control.

Example:

- Gmail.
- Microsoft 365.
- Salesforce.

## Easy Memory
- IaaS = I manage the OS.
- PaaS = I manage the app.
- SaaS = I just use the software.

## Scaling
### Vertical Scaling
Vertical scaling means making one server stronger.

Example:

Add more CPU or RAM.

Good for:

- Quick fixes.
- Small systems.

Limit:

One server can only grow so much.

### Horizontal Scaling
Horizontal scaling means adding more servers.

Example:

Run 5 web servers behind a load balancer.

Good for:

- Production systems.
- High availability.
- Handling traffic spikes.

## Cloud Security Basics
Important controls:

- IAM controls who can do what.
- Security groups control network access.
- Encryption protects data.
- MFA protects accounts.
- Logging helps investigation.
- Backups help recovery.

## Offensive Angle
Attackers look for cloud mistakes.

Common examples:

- Public storage buckets.
- Stolen access keys.
- Over-permissive IAM roles.
- Exposed management ports.
- SSRF to cloud metadata service.
- Disabled logging.

### SSRF to Metadata Example
This is a famous cloud attack pattern.

1. Web app has SSRF vulnerability.
2. Attacker forces server to request metadata URL.
3. Metadata service returns temporary credentials.
4. Attacker uses credentials to access cloud resources.

Example metadata URL:

```text
http://169.254.169.254/latest/meta-data/
```

## SOC Detection
Watch for:

- Unusual API calls.
- Login from strange location.
- Large storage downloads.
- New admin role creation.
- CloudTrail or audit logging disabled.
- Sudden compute spike from crypto mining.

## Interview Ready Answers
**What is the difference between IaaS, PaaS, and SaaS?**

IaaS gives the most control because you manage the OS and applications.
PaaS lets you manage application code while the provider manages the platform.
SaaS is ready-made software where the provider manages most of the system.

**What is horizontal scaling?**

Horizontal scaling means adding more servers or instances to handle more traffic.
It is common in cloud because it improves both performance and availability.

## Quick Revision
- Cloud = rented computing resources.
- IaaS = manage OS.
- PaaS = manage app.
- SaaS = use software.
- Horizontal scaling is preferred for cloud production systems.
- IAM mistakes are one of the biggest cloud risks.
