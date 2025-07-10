## ✅ Terraform Best Practices

* **Use modules**
  Structure and reuse infrastructure code using modules to keep it DRY and manageable.

* **Use remote backend and state locking**
  Store the Terraform state in remote storage like S3 and use DynamoDB for state locking to prevent concurrent updates.

* **Use provisioners only when necessary**
  Avoid using `local-exec` or `remote-exec` unless no other option exists. Prefer declarative resource creation.

* **Don't hardcode sensitive data**
  Store secrets in environment variables, `.tfvars` (excluded from version control), or use secret managers like AWS SSM or Vault.

* **Use `_` for internal resource names and `-` for exposed names**
  Example:

  * Internal: `aws_instance.web_server`
  * Exposed name: `web-server-prod`

* **Always run `terraform plan` before `apply`**
  Helps detect drift, preview changes, and catch mistakes early.

* **Scan Terraform configurations using tools like `tfsec`**
  Identify security vulnerabilities and misconfigurations before deployment.

---

## ✅ Terraform Best Practices

### 1. **Use Modules**

* Reuse code using modules to avoid duplication.
* Makes the code more organized and maintainable.
* Keeps root configuration clean and readable.

```hcl
module "vpc" {
  source = "./modules/vpc"
  cidr_block = var.vpc_cidr
}
```

---

### 2. **Use Remote Backend**

* Store the Terraform state file remotely (like in S3) to:

  * Share state across teams
  * Enable locking to avoid race conditions
  * Improve security and scalability

```hcl
terraform {
  backend "s3" {
    bucket = "my-tf-states"
    key    = "dev/terraform.tfstate"
    region = "us-east-1"
    dynamodb_table = "terraform-lock"
  }
}
```

---

### 3. **Avoid Overuse of Provisioners**

* Provisioners (like `remote-exec`, `local-exec`) should be used only as a last resort.
* They break the declarative model and can introduce instability.
* Prefer using proper resource definitions and user data.

---

### 4. **Don’t Hardcode Sensitive Data**

* Never hardcode secrets (like passwords, keys) in `.tf` files.
* Use:

  * Environment variables
  * `terraform.tfvars` (and don’t commit it)
  * Secrets Manager / Vault / SSM Parameter Store

```hcl
variable "db_password" {
  type = string
  sensitive = true
}

resource "aws_db_instance" "default" {
  ...
  password = var.db_password
}
```

