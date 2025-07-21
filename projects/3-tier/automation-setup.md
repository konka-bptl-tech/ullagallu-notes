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

# CI/CD

### CI Pipeline
stage:1 checkout
stage:2 npm test
stage:3 AMI build
stage:1 update LT with new AMI and Userdata
stage:2 Refreshs ASG with new AMI




Awesome, Konka! 🎉 Since you've completed a **3-tier architecture using VMs (EC2s managed via Auto Scaling Groups) and an ALB**, here's how you can implement **Blue-Green Deployment** using CI/CD (e.g., GitHub Actions or any tool) for the **backend tier**.

---

## ✅ Your Setup Recap (Now using EC2s):

### 3-tier Architecture

* **Frontend**: S3 + CloudFront (optional)
* **Backend**: EC2s (via Auto Scaling Group) behind an **Application Load Balancer (ALB)**
* **Database**: RDS (unchanged)

---

## 🎯 Goal: Blue-Green Deployment for Backend

Use **two Auto Scaling Groups (ASGs)** behind **two Target Groups**, with the **ALB forwarding traffic** only to the active one.

---

## ✅ Architecture for Blue-Green Deployment (VMs with ALB)

```
                     ┌────────────────────┐
                     │    Application     │
                     │  Load Balancer     │
                     └────────────────────┘
                         │           │
                ┌────────┘           └────────┐
        ┌─────────────┐              ┌──────────────┐
        │ Target Group│              │ Target Group │
        │   Blue      │              │   Green      │
        └─────────────┘              └──────────────┘
             │                             │
     ┌─────────────┐              ┌──────────────┐
     │  ASG (Blue) │              │  ASG (Green) │
     └─────────────┘              └──────────────┘
         EC2 v1.0                     EC2 v2.0
```

---

## 🛠️ Implementation Steps

### ✅ 1. Create Two ASGs (Blue & Green)

In Terraform:

* Create **two launch templates** (or versions)
* Create **two ASGs** with different names, pointing to different launch templates or AMIs.

### ✅ 2. Create Two Target Groups

* `blue-tg` → attached to ASG-Blue
* `green-tg` → attached to ASG-Green

### ✅ 3. ALB Listener Rule

* Default ALB rule forwards traffic to `blue-tg`
* During deployment, switch to `green-tg`

Example:

```hcl
resource "aws_lb_listener_rule" "app" {
  listener_arn = aws_lb_listener.http.arn
  priority     = 100
  action {
    type             = "forward"
    target_group_arn = aws_lb_target_group.green.arn
  }
  condition {
    path_pattern {
      values = ["/*"]
    }
  }
}
```

---

### ✅ 4. CI/CD Workflow (e.g., GitHub Actions)

#### 🎯 Goal: Automate these steps:

1. Create a new AMI (if needed)
2. Update the Green ASG’s launch template or launch configuration
3. Deploy the new app version to the Green ASG
4. Wait for health checks
5. Switch ALB target group to Green
6. Optionally terminate Blue after testing

---

## 🧪 Sample CI/CD Deployment Flow (Pseudocode)

```yaml
name: Blue-Green Deployment

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v3

    - name: Build and create AMI
      run: |
        packer build backend.pkr.hcl

    - name: Update Green Launch Template
      run: |
        aws ec2 create-launch-template-version --launch-template-id lt-xxxx --source-version 1 --launch-template-data '{"ImageId":"ami-xxxx"}'

    - name: Update Green ASG
      run: |
        aws autoscaling update-auto-scaling-group \
          --auto-scaling-group-name green-asg \
          --launch-template LaunchTemplateId=lt-xxxx,Version=$LATEST

    - name: Wait for instances to pass health checks
      run: |
        aws autoscaling wait instance-in-service --auto-scaling-group-name green-asg

    - name: Switch ALB listener to Green
      run: |
        aws elbv2 modify-listener \
          --listener-arn arn:aws:elasticloadbalancing:xxx \
          --default-actions Type=forward,TargetGroupArn=green-tg-arn

    - name: Optionally Terminate Blue ASG
      run: |
        aws autoscaling delete-auto-scaling-group --auto-scaling-group-name blue-asg --force-delete
```

---

## 🧠 Tips

* Use **launch templates with versions** for easy rollback
* Store AMI IDs in **Parameter Store or Terraform tfvars**
* Add **health check grace period** to avoid premature failures
* Monitor via **CloudWatch dashboards and alarms**

---
