# EKS Hosted
www.eks.konkas.tech (DNS)
    ↓
CloudFront (SSL Termination + CDN + WAF)
    ↓
ALB (Ingress Controller for EKS)
    ↓
EKS Cluster (Kubernetes)
    ↓
Frontend + Backend (Pod containers in EKS)
    ↓
Elasticache (Redis for caching)
    ↓
RDS (Relational Database)

CI/CD : Jenkins,ArgoCD
Docker: Containerization
Terraform: InfraStructure
Observability: Prometheus,Grafana,Elastic Stack,Kiali,Jaegar
HashiCorp Vault: Secrets management