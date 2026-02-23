# Implementation Notes

## Approach

Built network infrastructure templates using Terraform to standardize environment provisioning across development, staging, and production.

## Challenges Encountered

- Worked within IAM permission boundaries by adapting Flow Logs implementation
- Navigated Service Control Policy restrictions during infrastructure lifecycle
- Configured multi-account authentication with IAM Identity Center

## Key Design Decisions

- Single NAT for dev (cost optimization)
- Dual NAT for staging (balanced approach)
- Triple NAT for prod (full high availability)
- Database subnet isolation (no internet routes)
- S3 Gateway Endpoints (cost reduction)

## Lessons Learned

- Infrastructure as Code enables consistency and repeatability
- Cost optimization requires environment-appropriate design
- Security controls must be balanced with operational needs
- Multi-account strategies require careful authentication planning
