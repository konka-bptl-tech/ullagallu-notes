# Serverless-Hosted Architecture

```
www.serverless.konkas.tech (DNS - Route53 or similar)
    ↓
CloudFront (CDN + SSL + WAF + Caching)
    ↓
Frontend (ReactJS static files hosted on S3, CORS enabled)
    ↓
Internal ALB (Private)
    ↓
Backend (NodeJS app running on ECS - Fargate or EC2 launch type)
    ↘          ↘
   ElastiCache  ↓
        ↘     ↓
           RDS (Database)
```

- CI/CD : GithubActions
- Docker: Containerization
- Terraform: InfraStructure
- Observability: CloudWatch
- AWS Secrets Manager: Secrets management