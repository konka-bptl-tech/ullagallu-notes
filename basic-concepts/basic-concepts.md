# Idempotency
> Idempotency means if a resource doesn't exist, it will be created; if it already exists, it will be skipped — no matter how many times you run the script.
> This ensures that infrastructure changes are consistent, repeatable, predictable and safe to run multiple times.

# Drift
Drift means a difference between the actual state of your infrastructure and the expected state defined in your code or configuration.

Example (Terraform / Ansible Context):

* You define an EC2 instance in Terraform with a specific instance type: t3.medium
* Later, someone logs into the AWS console and manually changes it to t3.large
* Now, what’s running in AWS is different from what’s defined in Terraform — this is drift

Why Drift is a Problem:

1. Inconsistency across environments
2. Hard to debug issues (unexpected behavior)
3. Breaks automation and CI/CD pipelines
4. Security risks if unauthorized manual changes happen

How to Detect/Prevent Drift:

* In Terraform: Use `terraform plan` to see drift
* In CloudFormation: Use Drift Detection feature
* In general: Avoid manual changes, use immutable infrastructure

Interview Line You Can Use:

"Drift happens when manual or external changes cause infrastructure to differ from what’s defined in our IaC. We avoid this by using Packer and Terraform together with a strict no-manual-change policy."

# Mutable Infrastructure – Real Experience

Earlier, our application deployments were done using Ansible playbooks. The process looked like this:

1. Connect to each EC2 instance
2. Take backup
3. Stop the running application
4. Install the new version
5. Start the service

Since we are modifying live running servers, this is called mutable infrastructure.

Problems with Mutable Approach:

* Deployment issues – Out of 10 servers, maybe only 5 succeed due to network or permission issues
* Difficult rollbacks – Reverting back takes time and effort
* Drift – Configuration may differ between servers if some playbooks fail midway
* Risky – Application is tested after going live

Immutable Infrastructure Approach

With immutability, we don’t modify live servers. Instead, we:

* Build a golden image that contains:

  * OS
  * Application code
  * Required dependencies/libraries
* Test the image before release
* Deploy the image across all servers using Auto Scaling Group or Launch Template

Benefits of Immutability:

* Reliability
* Consistency across environments
* Eliminates drift
* Easy rollbacks (use previous AMI)
* Faster, safer releases

### Interview Line You Can Use:
We migrated from mutable infrastructure using Ansible to an immutable model with Packer. We build AMIs containing the OS, application, and dependencies, test them before release, and use them in auto scaling. This improved consistency, eliminated drift, and gave us faster rollbacks.



# DRY Principle (Don't Repeat Yourself)

The DRY principle stands for "Don't Repeat Yourself". It means avoid duplicating code or configuration. Instead, you should write reusable and modular code to improve maintainability and reduce errors.

Why DRY is important:

* Makes code easier to manage and update
* Reduces chances of bugs caused by inconsistent changes
* Promotes reusability and clean structure
* Saves time when scaling infrastructure or automation

Examples in DevOps:

1. Terraform – Use modules instead of writing the same resource code again and again
2. Jenkins – Use shared libraries instead of repeating pipeline steps in each job
3. Helm – Use templates and values files to avoid writing repetitive Kubernetes YAML
4. Ansible – Use roles and tasks to reuse configuration logic across multiple playbooks

Interview Line You Can Use:

"I follow the DRY principle in all my DevOps work. For example, I use Terraform modules to avoid repeating resource blocks, and shared libraries in Jenkins to reuse pipeline code. This makes our infrastructure more manageable, scalable, and consistent."


