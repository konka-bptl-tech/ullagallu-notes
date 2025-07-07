# VM based Architecture

```
www.vm.konkas.tech (DNS)
          ↓
CloudFront (CDN, SSL, WAF, Caching)
          ↓
External ALB (Public ALB)
          ↓
Frontend (ReactJS on VM, ASG)
          ↓
Internal ALB (Private ALB)
          ↓
Backend (NodeJS on VM, ASG)
      ↘      ↘
   ElastiCache  ↓
        ↘     ↓
           RDS (Database)

```
- CI/CD: Jenkins Pipelines
- Packer: AMI Build
- Terraform: Infra Automation
- Prometheus&Grafana: Metrics
- Elastic Stack: Logs
- AWS Secrets Manager: Secrets Management
---