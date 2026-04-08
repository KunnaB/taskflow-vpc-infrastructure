# AWS Multi-Environment VPC Infrastructure

**Three-tier VPC infrastructure across development, staging, and production environments using Terraform**

## Overview

Separate VPC configurations for each environment with isolated public, private application, and private database subnet tiers. Each environment uses a unique, non-overlapping CIDR range to allow future VPC peering.

## Architecture

### Development VPC
- **CIDR:** 10.0.0.0/16
- **Purpose:** Development and testing
- **Cost-optimized:** 1 NAT Gateway (us-east-1a)
- **Subnets:** Public, Private App, Private DB across 2 AZs

### Staging VPC
- **CIDR:** 10.1.0.0/16
- **Purpose:** Pre-production testing
- **High availability:** 2 NAT Gateways (one per AZ)
- **Subnets:** Public, Private App, Private DB across 2 AZs

### Production VPC
- **CIDR:** 10.2.0.0/16
- **Purpose:** Production workloads
- **High availability:** 3 NAT Gateways (one per AZ)
- **Subnets:** Public, Private App, Private DB across 3 AZs

## Repository Structure
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

## What Is Actually Provisioned

Each environment provisions:

- **VPC** with a unique, non-overlapping CIDR block
- **Public subnets** — for internet-facing resources (load balancers, bastion hosts)
- **Private App subnets** — for application tier (Lambda, EC2, ECS)
- **Private DB subnets** — for database tier (RDS, ElastiCache) with no internet route
- **Internet Gateway** — outbound internet for public subnets
- **NAT Gateway(s)** — outbound internet for private subnets (1 / 2 / 3 per environment)
- **Per-AZ route tables** for private app subnets (staging + prod) — ensures AZ-level fault isolation
- **VPC Flow Logs** — stored in S3 for network traffic auditing
- **S3 Gateway Endpoint** — free S3 access without NAT Gateway charges

## Features

- **3-Tier Architecture** — Isolated public, private app, and private DB tiers
- **Multi-AZ Design** — Subnets span 2 AZs (dev/staging) or 3 AZs (prod)
- **Unique CIDRs** — No overlap: Dev 10.0.0.0/16, Staging 10.1.0.0/16, Prod 10.2.0.0/16
- **NAT Gateway HA** — Staging uses 2 NAT GWs, production uses 3 (one per AZ)
- **VPC Flow Logs** — Network traffic monitoring stored in S3

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

## Cost Estimates

**Development:** ~$40/month (1 NAT Gateway)
**Staging:** ~$80/month (2 NAT Gateways)
**Production:** ~$120/month (3 NAT Gateways)

## Future Improvements

- **Security Groups** — Add least-privilege security groups for each tier (web, app, database)
- **VPC Peering / Transit Gateway** — Connect environments for shared services (e.g., centralised logging, bastion)
- **Terraform Modules** — Refactor the three similar configurations into a reusable VPC module

## Author

**Akunna Ndubuisi**
Solutions Architect | AWS Certified

## License

MIT License
