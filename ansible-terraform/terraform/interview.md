# Interview Questions
Here are **Terraform scenario-based questions** tailored for real-world DevOps use cases and interviews. These will test your **hands-on knowledge, problem-solving**, and **understanding of best practices**.

---

### ✅ **Basic to Intermediate Scenarios**

1. **Your team wants to provision infrastructure for dev, staging, and production environments. How would you structure your Terraform code to avoid duplication?**

   * (Hint: Modules, `backend.tf`, `*.tfvars`, and environment-specific workspaces or directories)

2. **You updated a Terraform module and ran `terraform apply`, but it deleted a resource unexpectedly. What steps would you take to debug and prevent this in the future?**

3. **You need to share Terraform state between team members. What solution would you implement and why?**

   * (Hint: Remote backend like S3 + DynamoDB for state locking)

4. **You made changes in the `main.tf` file, but after running `terraform plan`, nothing is detected. What could be the reasons?**

5. **How do you import existing AWS infrastructure (e.g., an EC2 instance) into your Terraform state file without recreating it?**

---

### ✅ **Advanced Scenarios**

6. **Your Terraform apply fails due to dependency errors. How do you identify and resolve these dependency issues?**

   * (Hint: `depends_on`, module relationships, circular dependencies)

7. **You are managing infrastructure using multiple modules and developers are making changes in parallel. How do you avoid Terraform state file conflicts?**

8. **You need to create a reusable module for deploying an ECS cluster. The module should be customizable (e.g., instance size, number of tasks). How would you design it?**

   * (Hint: Input variables, outputs, locals, version pinning)

9. **Your organization wants to enforce tagging policies (e.g., all resources must have `Environment`, `Owner`, and `Project`). How would you implement this using Terraform?**

10. **You have two modules: `network` and `compute`. `compute` depends on values (like subnet IDs) from `network`. How do you pass outputs from one module to another?**

---

### ✅ **CI/CD + Terraform Integration Scenarios**

11. **How would you integrate Terraform into a GitHub Actions pipeline for infrastructure-as-code deployments?**

* (Hint: Stages like `init`, `validate`, `plan`, `apply`, and approvals)

12. **You want to perform `terraform plan` on every PR and `terraform apply` only on merge to main. How would you configure this?**

13. **Terraform apply in the pipeline failed because of a drift in infrastructure. How do you investigate and handle this drift?**

---

### ✅ **Real-Time Problem Solving Scenarios**

14. **Your team has mistakenly deleted the Terraform state file. How do you recover and ensure no infrastructure is lost?**

15. **You deployed a VPC, EC2, and RDS instance, but now want to delete only EC2. How do you do this without affecting other resources?**

16. **You used hard-coded AMI IDs in your Terraform code. What are the risks, and how would you improve this setup?**

* (Hint: `data "aws_ami"` lookup, SSM parameters)

17. **You need to rotate AWS secrets periodically. How can you manage secrets rotation using Terraform along with AWS Secrets Manager?**

18. **Terraform fails due to API rate limits from AWS. How do you prevent or handle this situation efficiently?**

---
Q1. Terraform Fundamentals & How It Works
- It is a IaC tool developed by HashiCorp
- It uses declarative model allowing you to write concise configuration files
- that descrive all aspects of infra which could involve servers networking componentns,databases and storages entie application stack 
- When ever terraform apply commands fires tf looks into my configuration compares them with resources actually running in the cloud and then it will creates and modifies annd destroy aligned with real environment with declared in the code.
- Just declarae what you want terraform makes it reality
- Ensuring Consistency Scalability and repeatability at every single step

Q2. Terraform State File & Its Importance
- terraform statefile functions as a memory
- It keeps detailed record of every resources managed by terraform storing meta data like resources ids,configurations and dependencies every time you run terraform plan and apply 
terraform consults this file to determine which resources need to be created,modified or deleted So actual infrastructure matches what you declared in your .tf files state file also contain sensitive data keys or private configuration details which is why you should store in secure remote backend with features like locking and versioning this setup not only protects the file from accidental overrides but also prevents multple team members from making conflicting changes at the same time.

Q3. Let's We have resource in the cloud originally created outside terraform how do you bring it under terraforms management?
- If i discover a manually created cloud resource that i now want terraform to manage 
- My first step would be write a resource block in tf configuration that mirrors actual settings of that existing resources essentially describing what already exists next i use terraform import command specifying resource type internal terraform name real world resource identifier from the cloud provider the terraform import command updates terraform state file to refeclt these resource is no longer just out there unmanaged but officially recongnized and tracked by terraform from that point on ward any adjustments on that resource be it changes tags modifying size or even deleting will done through terraform minimizing the risk of drift or configuration mismatch in the feature



Q4. What is statefile accidentally gets deleted?How you handle that 
- If the statefile is lost terraform essentially forgets all the resources it previously managed If I've used remote backend with versioning enabled such as S3 I'll simply rollback prior version of the statefile that restores terraform memory which resources it has under management How ever if no backend ups exist then I've to re import each resources using terraform import so that terraform can rebuild the state from scratch this why it is imporatant to follow best practices like storing statefile in version backend and maintainning strict operational discipline to avoid major headaches when dealing with accidents or oversights

Q5. Could you explain how to resuse the same terraform configuration for multiple environments like dev,staging prod.......
- Build modules containning main resource definitions then supply each that is dev,stage,prod... with it's own set of input variables values for each environment this way every environement relias on the same code base but can adjut specific using variables 
- Another option is leverages the work spaces which allows you to maintain seperate files for each environment while still using same .tf configurations both methods to keep my code DRY consistent and easy to manage 

Q6. What is the difference between terraform plan and apply?
- Terraform plan allows you to preview the exact changes terraform intends to make like many resources are created,modifies or destroyed without committing any actions this preview step is crtical helps you to identify potential issues or unitended consequences before they impact live infrastructure then you're confident about you plan
- Terraform apply to actually implement those changes bringging your environment into alignment with declared state using terraform apply catch our mistakes before they become permanent

Q7. How do you collaborate on terraform projects with a much larger team?
When multiple people need to update the same terraform configurations I never relay on local state file for anything missin-critical or productions related Instead i store the statefile in a remote backend such as aws s3 terraform cloud or azure blob so everyone refer to the  same autoortive sources I also ensure that state locking enable prevents 2 team members from running terraform apply at the same time and accidentally overriding each other changes on top of that encryption for the stored state as well as access controls role based permissions ensure only right people can modify or view the state file this setup drastically reduces the risk of concurrency conflicts acciental deletions or data corruptions 

Q8. If terraform has created 30 ec2 instances but you only remove single instance how would you do that without effected the rest?
By single command that is
terraform destroy -target=aws_instance.server20 define exact resource you want retire this instructs terraform only destroy that particular instance while leaving everything else vital capability for those moments when you need a partial decommissioning 

Q9. How would you securely store database credentials or API tokens when working with terraform?
Never acceptable to hard code sensitive info direclty into the tf files or commit such data any kind of VCS instead we would use SecretsManager HashiCorp Vault use environment variables set them at runtime terraform treats input as variables and never exposes them in logs in some workflows you might store these secrets as workspaces variables in terraform cloud or CI/CD mark any critical variable as sensistive values display in plan and apply

Q10. How would you handle situation where you need to create resources in both AWS and Azure With in same terraform configuration?
You can define multiple provider blocks one for AWS and another for Azure each contain its own auth and configuration settings such as region or subscription details from there each resource block specifies which provider to use wheather provider equals aws or provider equals azure ensuring terr

Q11. How would you integrate terraform with CI/CD pipeline?
We typically store all out tf configuration in VCS every time new commit is push CI/CD pipeline is triggered starting with init download required plugins and prepare working dir and next validate plan and apply creating end to end automation process for infra creation i.e is versioning,reviewable and auditable

Q12. What happends if someone makes changes directly in the cloud console instead of going through terraform?
In our organization we ideally restrict manaul  modifications in production so no one should be tweaking resources outside terraform process however if manual changes do occur you need a strategy for drift detection which typically involves running terraform plan on a regular schedule when terraform detects mismatch between what's defined in your tf files and actual infra state if flags those dependencies you then decide wheather to rollback the changes by reapplying the code effectively overwriting any console change or update configuration if the console modifications are valid and you want them to persist 

Q13. Why Use terraform modules and what advantages do they offer in large sclae projects  
 
Q14. When would you choose count over for_each vice-versa?
Q15. How do terraform workspaces help in managing different environments and when should you use them
Q16. Can you walk me through how you'd achieve a blue-green deployment using terraform
Q17. How do create_before_destroy and prevent_destroy help in prodution systems?
Q18. How do you properly test terraform code
Q19. What do you do if you need to migrate from terraform 1.5 to 1.10?
Q20. If we rely on terraform to provision servers how do we integrate ansible for application setup?
