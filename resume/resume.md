https://www.youtube.com/watch?v=Lpz1ua3n_bU
https://www.youtube.com/watch?v=vHd18cwh-iE&t=235s
https://www.youtube.com/watch?v=V0Scaxu9fL4
https://www.youtube.com/watch?v=lBaK6Fq1QJY
https://www.youtube.com/watch?v=C3pgRZuz2VM
https://www.youtube.com/watch?v=q5hlndIHcys



"We have an application that allows users to report issues or problems they experience with their car, such as mechanical failures, warning lights, or strange noises. The app helps diagnose the problem or connects the user with support or service providers."



It is 3-tier application
- Frontend[ReactJs]
- Backend[SpringBoot]
- Redis Cache
- MySql DB


Hosted in AWS

Application Flow is like that
                                                                                                               Elastic Cache[Redis]
                                                                                                                 /
                                                                                                                /
www.mahindra-car.com  --> cloud front --> External ALB --> Frontend VM[ASG] --> Internal ALB --> Backend VM[ASG]
                                                                                                                \
                                                                                                                 \
                                                                                                                RDS[MySql]
AWS Services Used in this project:
----------------------------------
1. VPC
2. EC2, ALB , ASG
3. Elastic Cache[Redis]
4. RDS
5. ParamterStore,S3,Secrets Manager
6. Cloud Trail
7. IAM 
8. Private Certificate Manager
9. WAF, Cloud Front
10. Route53
11. CloudWatch

CI/CD pipelines for deployment
1. Jenkins[CI/CD]
2. Packer for AMI build

Terraform for infra management

As part of the infra modernization we migrate vm based hosting to complete serverless


                                                                     Elastic Cache[Valkey]
                                                                            /
                                                                           /
www.mahindra-car.com  --> cloud front --> s3[cors] --> ALB --> Backend[ECS]
                                                                           \
                                                                            \
                                                                         RDS[MySql]


AWS Services Used in this project:
----------------------------------
1. VPC
2. ALB, Private Certificate Manager
3. Elastic Cache[Valkey]
4. RDS[MySql]
5. Parameter Store, Secrets Manager
6. Cloud Trail
7. IAM
8. Guard Duty,WAF
9. Cloud Front
10. S3, ECS,ECR
11. Cloud Watch
12. Route53


This reduce costs by 30% it self only valkey and give 10x faster than redis

- Migrated Jenkins pipelines to GitHub actions and setup ECS Dynamic Containers for agents

𝗥𝗼𝘂𝗻𝗱 𝟭: 𝗧𝗲𝗰𝗵𝗻𝗶𝗰𝗮𝗹 𝗦𝗰𝗿𝗲𝗲𝗻𝗶𝗻𝗴
1. Explain the CI/CD workflow you follow and the kind of pipeline you use. How do you define and invoke pipelines in Jenkins?
2. What are shared libraries in Jenkins, and how are they written and defined?
3. What kind of applications do you deploy using Jenkins pipelines, and what deployment tools do you use?
4. If the Jenkins pipeline runs but the build doesn’t happen, what possible issues could be causing it?
5. What is the purpose of a webhook, and how is it used in a CI/CD pipeline?
6. How do you create and manage Kubernetes clusters (using tools like Terraform), and what are the master and worker nodes?
7. What are common Kubernetes errors you’ve faced (like CrashLoopBackOff, ImagePullError), and how did you resolve them?
8. What is the command to access a pod and how can you define or create a Kubernetes class or object?
9. Explain the folder structure of a basic Helm chart. What commands do you use to deploy with Helm?
10. What are the stages in a Docker image build? Why do we use ENTRYPOINT and CMD instructions?
11. How do you manage and connect services like DBs, EC2, EKS, or ECS? Include the command to connect to ECS.
12. Which container registry do you use for storing Docker images?

𝗥𝗼𝘂𝗻𝗱 𝟮: 𝗜𝗻-𝗱𝗲𝗽𝘁𝗵 𝗧𝗲𝗰𝗵𝗻𝗶𝗰𝗮𝗹 𝗦𝗰𝗿𝗲𝗲𝗻𝗶𝗻𝗴
1. What branching strategy do you follow, and how do you handle merges to avoid breaking the release branch? If a bug appears in production, what’s your approach to resolving it?
2. Describe your typical deployment flow and CI/CD workflow. What stages do you define in your Jenkins pipeline, and how do you ensure full quality checks during deployment?
3. How do you use Jenkins shared libraries? Explain their typical structure and how they are integrated into your Jenkinsfiles.
4. Are you aware of security scanning tools? How do you scan Docker images—both during build and at the registry level? Are you using any extensions or tools for image scanning?
5. How do you pass environment variables during Docker build commands? What services do you use for storing Docker images?
6. How do you establish a connection with databases in your deployments or infrastructure setup?
7. How do you handle authentication for EKS clusters and store secrets securely in your environment?
8. How do you create AWS Lambda functions and manage the artifacts for deployment? What options do you use to push artifacts to Lambda?
9. What is email signing and Helm chart signing? Which tools do you use to sign Helm charts?

𝗥𝗼𝘂𝗻𝗱 𝟯: 𝗛𝗠 𝗥𝗼𝘂𝗻𝗱 (𝗛𝗶𝗿𝗶𝗻𝗴 𝗠𝗮𝗻𝗮𝗴𝗲𝗿)
1. Project experiences.
2. Day-to-day responsibilities.
3. Light behavioral questions.
4. Teamwork & Culture Fit questions.



vajpayee
advani
defence minister george fernandes
pramod mahajan
jaswanth singh
yashwanth sinha
brijesh mishra principle secratary of prime minister take care about coordination with scientists 


Time Line[Jan'2021 to Feb'2025]
E-Commerce Project have 70 microservices

Languages: Java,Nodejs,Python and ReactJs,Mysql, Redis ,Rabbit Mq and Mongo

Cloud: AWS
Tools: Terraform, Docker ,K8s, Jenkins, GitHub Actions , ArgoCD, Linux, Bash Scripting, Prometheus & Grafana ,Elastic Stack , Istio, Helm,EKS

Responsible for 
- Cluster Upgradation
- Migrate EBS volumes from gp2 to gp3 using K8s manifest files
- Write CI/CD pipelines implement security into every aspect is Devops practices
- Write terraform modules


Time Line[Feb'2021 to current data]
-----------------------------------
I have worked on simple 3-tier application was hosted on 

VM application workflow
www.mahindra.com --> Cloud Front --> External ALB --> Frontend[ASG] --> Internal ALB --> Backend[ALB]
                                                                                                     --> Elastic Cache[redis]
                                                                                                     --> RDS[MySql]
AWS services used in this project
---------------------------------
1. VPC and Components
2. EC2,EBS,ELB,ASG
3. IAM,Route53,CloudFront
4. Secrets Manager, Parameter Store
5. CloudWatch
6. Cloud Trail, WAF, Guard Duty
7. Private Certificate Manager

CI/CD pipeline Structure
------------------------
1. Jenkins
2. Packer[Immutable Releases]

Programming lang and Cache and Databases
1. ReactJS
2. Spring Boot
3. Redis
4. MySql

Infrastructure Management Tool
Terraform

We got the task that is need to migrate 3-tier vm to complete serverless architecture

Serverlass application flow

www.mahindra.com --> Cloud Front --> S3[CORS & Cloud Front] --> ECS[Backend]
                                                                           - Elastic Cache[valkey]
                                                                           - RDS[MySql]
I did POC about application migration

We have one Sample application first I check workflow from end-to-end testing it's working fine I give the demo about my POC to my client they were impressed with my work

Then later I'll start working terraform to architect Infra for serverless infra

We Would also migrate Jenkins pipelines in to Githubactions workflows and use ECS Dynamic Agents for building CI/CD pipelines 
We would also Migrate Infra pipelines into Githubactions

It would take near 12 to 15 weaks for us to migrate application into vms to complete serverless

Before my project migration 1 month back they introduce valkey it offers 30% lesser costs than redis oss and offer 4x faster than redis

