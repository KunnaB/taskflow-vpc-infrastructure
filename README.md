# AWS Multi-Environment VPC Infrastructure

**Enterprise-grade multi-environment VPC architecture using Terraform**

## Overview

Three-tier VPC infrastructure designed for enterprise workloads across development, staging, and production environments.

## Architecture

### Development VPC
- **CIDR:** 10.0.0.0/16
- **Purpose:** Development and testing
- **Cost-optimized:** Single NAT Gateway
- **Subnets:** Public, Private App, Private DB (2 AZs)

### Staging VPC
- **CIDR:** 10.1.0.0/16
- **Purpose:** Pre-production testing
- **High availability:** 2 NAT Gateways
- **Subnets:** Public, Private App, Private DB (2 AZs)

### Production VPC
- **CIDR:** 10.2.0.0/16
- **Purpose:** Production workloads
- **High availability:** 3 NAT Gateways
- **Subnets:** Public, Private App, Private DB (3 AZs)

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

## Features

✅ **3-Tier Architecture** - Isolated public, private app, and private DB tiers  
✅ **Multi-AZ Design** - High availability across availability zones  
✅ **Security Groups** - Least-privilege access controls  
✅ **NAT Gateways** - Secure outbound internet access  
✅ **VPC Flow Logs** - Network traffic monitoring  
✅ **Terraform Modules** - Reusable, maintainable code  

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

## Author

**Akunna Ndubuisi**  
Solutions Architect | AWS Certified  

## License

MIT License
