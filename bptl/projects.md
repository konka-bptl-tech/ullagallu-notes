# 3-tier project setup
### Manual Setup
1. RDS creaation dns record creation and load the schema
2. Elasti Cache Creation create the DNS record and test it connection
3. Build the backend AMI using packer
  - create environment variables in parameter store
  - place the secrets in Secrets Manager
  - Create IAM policy to get access paramter store and secrets manage and also create IAM role and attach IAM policy to Role
4. Create Launch Template choose AMI as Above create AMI
5. Create Target Group for bacnekd and create ALB and in listner 80 forward traffic to backend tg
   - Create DNS record for ALB
6. Create backend ASG and choose existing ALB
   - test it endpoint
7. Build frontend AMI using packer
   - Create S3 bucket and place nginx.conf
   - Create IAM policy to read s3 bucket and create IAM Role and attach the policy
8. Create Lauch Template use as above image
9. Create TG for frontend and create ALB with 80 listener forward traffic to frontend TG
   - create ACM certificate and update records in R53 and 443 listner in frontend ALB forwards traffic to frontend TG
   - edit the 80 listner redirects to 443
10. Create ASG for frontend choose existing LB
11. Create Cloud Front Distribution origin as frontend DNS

### Terraform setup
1. terraform folder structure overview
```markdown
|-- .github
|   `-- workflows
|       |-- destroy.yml
|       `-- provision.yml
|-- 3-tier.drawio.svg
|-- README.md
|-- data.tf
|-- environments
|   `-- development
|       |-- backend.tfvars
|       `-- main.tfvars
|-- main.tf
|-- modules
|   |-- sg
|   |   |-- main.tf
|   |   |-- outputs.tf
|   |   `-- variables.tf
|   `-- vpc
|       |-- main.tf
|       |-- outputs.tf
|       `-- variables.tf
|-- outputs.tf
|-- provider.tf
|-- sg-rules.drawio.svg
|-- sg.tf
`-- variables.tf
```
2. Develop VPC module use in modules and pass the values in devlopment modules
3. Same way in all remainning modules
................

### SetUp CI/CD pipeline to automate deployment
1. Jenkins Overview why we are using Jenkins
2. Jenkins Master Slave setup
3. Setup CI/CD pipelines for auto deployment

# ECS Serverless project setup
### Manual Setup with AWS CLI
0. Decide 2 Records for backend and Cloud Front
1. Create ECS cluster
2. Create Cloud Map Ns
3. Create cloudwatch log group
4. Create Parameterstore to place environment variables and secrets for secrets manager
5. Create IAM policy to access
   - Paramters store
   - Secrets Manager
   - ECS Task execution
6. Build Docker Image
7. Create Task Definition
8. Create Service
9. Create TG and ALB add listners 80,443 and acm certificate
10. Build Frontend Application npm run build
11. Create S3 bucket upload npm run build files in to S3 buacket
12. Create CloudFront Enable CORS update bucket policy

### Terraform way to setup infra like above 3-tier
- follow same structure like above

### SetUP CI/CD pipelines using github actions

# EKS Project Setup
### Application Deployment on EKS
- expense[containerize]
- instana[containerize]

### Terraform way to setup infra for EKS

### Using Jenkins for CI and ArGoCD for Deployment



















# ECS Project Setup


