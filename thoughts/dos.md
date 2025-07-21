# 3-tier project
- setup infra for 3-tier project using terraform[completed]
- setup CI/CD pipelines for 3-tier project using jenkins
- Blue-Green Deployment
- CloudWatch Integration get logs of backend and frontend as well as memory usage and everyting 
  nof request...............

# Migration to Serverless
- setup infra for serverless using terraform
  - iam
  - vpc
  - sg
  - bastion
  - alb,acm
  - ecs
  - s3,cloudfront
  - cloudwatch
- setup CI/CD for serverless using githubactions
- deploying application using blue-green strategy

# EKS project
- setup infra for eks using terraform
  - vpc[c]
  - sg[c]
  - eks[c]
    - controlplane[c]
    - nodegroup[c]
    - addons[c]
    - access entry[c]
    - bastion
    - pod identities[c][successfully check ebs example]
    - helm charts
    - karpenter
    - istio

- setup CI/CD for EKS using Jenkins and ArgoCD
- Deploying application using blue-green stratey and rolling update
- Deploying application using canary-deployments
- Velero backup and Cluster Upgradation

# Key words
SAST,DAST,SCA