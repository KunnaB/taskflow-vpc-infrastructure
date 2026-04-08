# TaskFlow - Multi-Environment AWS Infrastructure

**Enterprise-grade AWS network infrastructure demonstrating production-ready VPC architecture across development, staging, and production environments**

## Problem Statement

*"We're a growing tech company with 15 development teams. Each team needs their own AWS environment (dev/staging/prod). Currently, our platform team spends 8 hours per week manually setting up environments - creating VPCs, launching EC2 instances, configuring databases, setting up monitoring.*

*Teams complain it takes 2-3 days to get an environment. We're also seeing AWS bills climb because teams forget to shut down dev resources. Security is concerned because configurations are inconsistent.*

*We need a self-service platform where teams can request environments and get them provisioned automatically with proper security, monitoring, and cost controls."*

## Solution

I designed and implemented automated infrastructure templates using Terraform to provision complete AWS environments with:

- Multi-tier VPC architecture (public, application, and database tiers)
- High availability across multiple availability zones
- Cost optimization strategies tailored to each environment
- Comprehensive security controls and monitoring

## Architecture Overview

### Visual Architecture Diagrams

**Development VPC**

![Development VPC Architecture](docs/architecture/dev-vpc-architecture.png)

*Development environment with cost-optimized single NAT Gateway configuration*

**Staging VPC**

![Staging VPC Architecture](docs/architecture/staging-vpc-architecture.png)

*Staging environment with dual NAT Gateways for partial high availability*

**Production VPC**

![Production VPC Architecture](docs/architecture/prod-vpc-architecture.png)

*Production environment with triple NAT Gateways for full high availability across 3 AZs*

### Three-Tier Network Design

Each environment follows a standardized three-tier architecture:

**Public Tier**
- Internet-facing resources
- Load balancers and NAT gateways
- Direct internet gateway connectivity

**Application Tier**
- Private subnets for application workloads
- Outbound internet access via NAT gateway
- Isolated from direct internet exposure

**Database Tier**
- Fully isolated private subnets
- No internet connectivity (inbound or outbound)
- Communication only within VPC boundaries

### Environment-Specific Implementations

**Development Environment**
- **CIDR:** 10.0.0.0/16
- **Availability Zones:** 2 (us-east-1a, us-east-1b)
- **NAT Gateways:** 1 (cost-optimized)
- **Purpose:** Development and testing workloads

**Staging Environment**
- **CIDR:** 10.1.0.0/16
- **Availability Zones:** 2 (us-east-1a, us-east-1b)
- **NAT Gateways:** 2 (partial high availability - one per AZ)
- **Purpose:** Pre-production testing environment

**Production Environment**
- **CIDR:** 10.2.0.0/16
- **Availability Zones:** 3 (us-east-1a, us-east-1b, us-east-1c)
- **NAT Gateways:** 3 (full high availability - one per AZ)
- **Purpose:** Enterprise-grade production workloads

## Technical Implementation

### Infrastructure as Code

All infrastructure defined using Terraform with:
- Modular, reusable resource definitions
- Variable-driven configuration for environment customization
- State management with remote backends
- Comprehensive output values for resource references

### What Is Actually Provisioned

Each environment provisions:

- **VPC** with unique, non-overlapping CIDR block
- **Public subnets** — Internet-facing resources (load balancers, bastion hosts)
- **Private App subnets** — Application tier (Lambda, EC2, ECS)
- **Private DB subnets** — Database tier (RDS, ElastiCache) with NO internet route
- **Internet Gateway** — Outbound internet for public subnets
- **NAT Gateway(s)** — Outbound internet for private subnets (1/2/3 per environment)
- **Per-AZ route tables** — Staging and prod use zone-specific routing for fault isolation
- **VPC Flow Logs** — Stored in S3 for network traffic auditing
- **S3 Gateway Endpoint** — Free S3 access without NAT Gateway data transfer charges

### Security Features

**Network Isolation**
- Database subnets have no route to internet gateway or NAT
- Defense-in-depth architecture prevents unauthorized data exfiltration
- Network segmentation follows AWS Well-Architected Framework

**Monitoring and Compliance**
- VPC Flow Logs capturing all network traffic
- Centralized logging to S3 for audit and analysis
- CloudWatch integration for operational visibility

**Access Control**
- IAM Identity Center integration for centralized authentication
- Service Control Policies enforcing organizational guardrails
- Least-privilege access principles throughout

### Cost Optimization

**Strategic NAT Gateway Deployment**
- Development: Single NAT gateway reduces operational costs (~$40/month)
- Staging: Dual NAT balances cost and availability (~$80/month)
- Production: Triple NAT provides maximum resilience (~$120/month)

**VPC Endpoints**
- S3 Gateway Endpoints eliminate NAT data transfer costs
- Free gateway endpoints provide direct AWS service access

**Resource Tagging**
- Comprehensive tagging strategy for cost allocation
- Environment-specific budget tracking
- Team and application-level cost visibility

## Project Structure

```
multi-vpc-project/
├── terraform/
│   ├── dev-vpc/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   ├── staging-vpc/
│   │   ├── main.tf
│   │   ├── variables.tf
│   │   └── outputs.tf
│   └── prod-vpc/
│       ├── main.tf
│       ├── variables.tf
│       └── outputs.tf
└── docs/
    └── architecture/
        ├── dev-vpc-architecture.png
        ├── staging-vpc-architecture.png
        └── prod-vpc-architecture.png
```

## Quick Start

### Prerequisites
- AWS Account
- Terraform >= 1.0
- AWS CLI configured

### Deploy Development VPC
```bash
cd terraform/dev-vpc
terraform init
terraform plan
terraform apply
```

### Deploy Staging VPC
```bash
cd terraform/staging-vpc
terraform init
terraform plan
terraform apply
```

### Deploy Production VPC
```bash
cd terraform/prod-vpc
terraform init
terraform plan
terraform apply
```

## Future Improvements

- **Security Groups** — Add least-privilege security groups for each tier (web, app, database)
- **VPC Peering / Transit Gateway** — Connect environments for shared services (e.g., centralised logging, bastion)
- **Terraform Modules** — Refactor the three similar configurations into a reusable VPC module

## Author

**Akunna Ndubuisi**
Solutions Architect | AWS Certified

## License

MIT License
