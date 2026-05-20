# Lecture 24: Infrastructure as Code (IaC)
Date: May 20, 2026
Source: NetworkChuck Free CCNA

## What is IaC?
Managing infrastructure using code instead
of manual GUI/CLI configuration.
Infrastructure = repeatable, automated,
version controlled.

## Traditional vs IaC
| Feature | Traditional | IaC |
|---------|-------------|-----|
| Setup | Manual GUI/CLI | Code files |
| Speed | Slow | Fast |
| Errors | Human mistakes | Automated |
| Scaling | Difficult | Easy |
| Tracking | Hard | Git history |

## IaC Types
| Type | Approach | Examples |
|------|----------|----------|
| Declarative | Define WHAT you want | Terraform, CloudFormation |
| Imperative | Define HOW to build it | Bash, Ansible |

## Popular IaC Tools
| Tool | Use | Language |
|------|-----|----------|
| Terraform | Multi-cloud provisioning | HCL |
| Ansible | Config management | YAML |
| CloudFormation | AWS only | JSON/YAML |
| Puppet | Automation | Ruby DSL |
| Chef | Infrastructure automation | Ruby |

## Terraform Workflow
```bash
terraform init      # Initialize
terraform plan      # Preview changes
terraform apply     # Create infrastructure
terraform destroy   # Remove infrastructure
```

## Example Terraform Code
```hcl
resource "aws_instance" "web" {
  ami           = "ami-123456"
  instance_type = "t2.micro"
  tags = {
    Name = "WebServer"
  }
}
```

## Example Ansible Playbook
```yaml
- name: Install Apache
  hosts: webservers
  tasks:
    - name: Install Apache
      apt:
        name: apache2
        state: present
```

## Offensive Angle
| Attack | Description |
|--------|-------------|
| Hardcoded secrets | API keys in Terraform files = credential theft |
| Public S3 buckets | Misconfigured IaC exposes storage |
| Overprivileged IAM | IaC creates admin roles unnecessarily |
| Git history | Deleted secrets still in git log |
| Supply chain | Malicious Terraform modules |
| CI/CD pipeline | Compromise pipeline = compromise all infra |

### Hardcoded Secrets — Most Common IaC Mistake
```hcl
# WRONG — Never do this
resource "aws_instance" "web" {
  access_key = "AKIAIOSFODNN7EXAMPLE"
  secret_key = "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
}

# Attackers search GitHub for these:
# site:github.com "aws_secret_key"
# site:github.com "terraform.tfvars"
# site:github.com "AKIA" extension:tf
```

### Git History Attack
```bash
# Developer commits secrets accidentally
# Deletes the file — but git remembers everything

git log --all --full-history
git show <commit-hash>
# Secrets visible in old commits

# Tool: TruffleHog scans git history
trufflehog git https://github.com/target/repo
```

### Malicious Terraform Module
```hcl
# Attacker publishes "helpful" module
# Developer uses it blindly:
module "network" {
  source = "attacker/malicious-module/aws"
  # Module secretly:
  # - Creates backdoor IAM user
  # - Exfiltrates credentials
  # - Opens security groups
}
```

### CI/CD Pipeline Compromise
Developer pushes IaC code
Pipeline runs terraform apply
If pipeline is compromised:

Attacker modifies IaC before apply
Creates backdoor resources
Exfiltrates cloud credentials
Full cloud environment compromise

## SOC Detection
| Attack | Detection |
|--------|-----------|
| Hardcoded secrets | TruffleHog/GitLeaks in CI/CD pipeline |
| Misconfigured resources | AWS Config, Azure Policy alerts |
| Unauthorized IAM | CloudTrail: new admin role created |
| Public bucket | S3 public access alert |
| Pipeline compromise | Unexpected infrastructure changes |
| Config drift | Terraform plan shows unplanned changes |

### IaC Security Scanning Tools
```bash
# Scan Terraform for misconfigs
tfsec .
checkov -d .

# Scan for secrets in code
trufflehog filesystem .
gitleaks detect

# AWS IaC misconfiguration
aws config get-compliance-details-by-config-rule
```

### Key SOC Rules for IaC
Rule 1: New IAM admin created outside pipeline = alert
Rule 2: S3 bucket made public = critical alert
Rule 3: Security group opens 0.0.0.0/0 = alert
Rule 4: Terraform state file accessed externally = alert
Rule 5: CI/CD pipeline runs outside business hours = flag

## IaC Best Practices
| Practice | Why |
|----------|-----|
| Never hardcode secrets | Use Vault or env variables |
| Use least privilege IAM | Limit blast radius |
| Scan before apply | tfsec, checkov |
| Version control everything | Git for all IaC |
| Review pipeline access | Who can trigger terraform apply? |
| Use remote state | Encrypted S3 + DynamoDB locking |

## Interview Questions
**Q: What is configuration drift?**
Infrastructure diverges from intended state.
Happens when manual changes made outside IaC.
Solution: immutable infrastructure — replace
don't modify. Terraform detects drift with
terraform plan showing unexpected changes.

**Q: Declarative vs Imperative IaC?**
Declarative = define desired end state.
Tool figures out how to get there.
Terraform, CloudFormation.
Imperative = define exact steps.
Ansible playbooks, Bash scripts.

**Q: Biggest IaC security risk?**
Hardcoded secrets in code files.
Developers commit AWS keys, passwords.
Even deleted files visible in git history.
Use TruffleHog to detect. Use Vault to fix.

**Q: What is immutable infrastructure?**
Never modify existing infrastructure.
Replace entirely when change needed.
Benefits: consistency, easy rollback,
no configuration drift.
