# Interview-1
1. Tell me about yourself
2. What's your previous project experience
3. On which tools you're working
4. Difference between blue-green and canary deployments
5. Can you share your screen can you write deployment yaml
6. Difference between replicaset and deployment
7. How would you configure HPA
8. Which Monitoring tool you're used
9. How pods are automatically scale on which parameter
10. Can you please explain docker architecture
11. If daemon process is not responding as a devops engineer what you will do
12. In ansible what is the difference between pull based and push based architecture
13. Sample playbook to upgrade java version only redhat based machines
14. Can you please explain dynamic inventory and static inventory
15. What are the variables in ansible
16. If you loose the terraform statefile what will happen what are the problems that we are facing how can you prevent from accidental deletion or loss
17. Can you please explain about variables in terraform
18. terraform modules
19. Can you please explain about maven life cycle
20. Recently I clone the code somany sub dirs and files we have a file in some location How can i identify that file
21. We have 100 files can you find maximum disk utilized file identify
22. VPC peering
23. What types of instances are you using

# Interview-2
1. Tell me about yourself explain roles and repsonsibilites?
2. What was the cause and solution when a pod remained in pending state in your cluster?
3. How PV and PVC working in your project how it is behaving are you setup from scratch
4. Diffeence between liveness and readiness probe how did you configure them in your project
5. Can you explain HPA in Pod Cluster
6. What steps did you take when a docker container was running but the application inside was not accessible
7. What is the difference between CMD and entrypoint
8. How do you optimize docker images for faster build and push in CI/CD
9. Can you please share yourscreen and write Docker Compose file for mongo DB
10. Can you explain me what are plugins you installed in your jenkins CI/CD pipeline
11. Can you write a one declarative script
12. How many build agents there in your project what are they capacity
13.  Difference between reset and revert actual use case
14. Write a shell script to archieve logs older than 7 days send an alert if disk usage exceedds 90%
15. Hard and soft links
16. What is the life cycle of terraform
17. In you project how do you ensure the terraform changes are reviewed and approved before apply
18. state locking
19. VPC CIDR blocks
20. ELB
21. eliminate duplicate elements

# Interview-3
1. Tell me about your current devops project and what are your daily roles and responsibilities
2. How do you identify and resolve the high cpu and memory usage in a linux server
3. Can you please explain me a situation where you handle git merge conflict during a hot fix in a live production branch
4. How do you enforce branch protection and code reviews in github CI/CD pipelines
5. What are the strategies you have used to reduce a docker image size and speedup the build time
6. How do you handle the persistent data in a docker contaienr
7. How do you argoCd fit to gitops workflow in CI/CD pipeline
8. How can you configure HA and Scalabity in AWS
9. VPC peering
10. terraform taint and untaint and refresh
11. How did you handle situation where k8s prod was going to crashloopbackoff state




# Interview-4
1. Structure GitHub Actions workflows to support multi-service deployment with rollback
2. Perform canary deployments using Helm and Argo Rollouts
3. Securely inject API keys in GitHub Actions without exposing them in logs
4. Efficient release versioning across microservices
5. Handle CPU throttling in a busy AKS/EKS cluster under payment traffic load
6. Readiness gates in Kubernetes and when to use them
7. Debug HPA not scaling pods despite high CPU usage
8. Ensure blue-green or shadow deployment testing for critical services
9. Structure Terraform code for a multi-region, multi-account AWS setup
10. Tagging and cost governance across cloud infrastructure
11. Rotate AWS IAM credentials and ensure services update them dynamically
12. Benefit of using for\_each over count in complex Terraform modules
13. Trace spike in failed transactions to app logic, infra failure, or third-party API
14. Build actionable alerts and reduce false positives in PagerDuty/Prometheus
15. Implement distributed tracing in a Kubernetes-based microservices system
16. Prevent hardcoded secrets in Terraform and Dockerfiles
17. Enforce least privilege access in a CI/CD pipeline
18. Manage vulnerability scanning and patching in high-frequency deployments
