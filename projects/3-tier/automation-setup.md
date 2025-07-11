# InfraSetup using Terraform
1. VPC & Components
2. SG
3. EC2
4. RDS
5. Elastic Cache
6. ACM
7. ALB
8. IAM
9. ASG
10. CloudFront

- Required aws services to setup infra for 3-teir application which is scalable and highly available
- I leverage the terraform modules to reuse infra code again and again

# Jenkins
- Automate build and release process

# Logging & Monitoring
- Prometheus & Grafana
- ELK stack



# Easy to Setup
Today Need to do
- VPC
- SG & Rules
- Bastion & VPN
- RDS
- Elastic Cache
- ALB & TG
  - Internal & External
- ASG
  - frontend & backend

- IAM Roles & Polcies[Do manually]
- Parameter Store Manully
- AWS Secrets Manager Manually
Why becuase I wasted so much time on this once my 90% automation completes I will be work out on above Components

- CI/CD pipelines to Auto Release Application

