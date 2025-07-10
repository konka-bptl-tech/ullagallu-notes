# Terraform concepts
- Introduction about before and after IaC and Terraform
- Terraform blocks
  - terraform settings block
  - provider block
  - resource block
  - variable block
  - output block
  - data block
  - null resource
  - provisioners

- Practices Session
  - Create EC2 instance in AWS
    - using settings block to tells which provider going to use
    - using provider block to pass auth configuration
    - using data block pull the AMI
    - using resource block define ec2 instance
    - using variables block avoid hardcoding values
    - using output block to print ec2 public and private IP's
    - use null resource to pass script
      - use file provisioner to copy script from local to ec2 machine
      - use remote provisioner to execute the script in ec2 machine
      - use local provisioner to print any message on local

- terraform commands
  - init,fmt,validate,plan,apply,destroy,state list

- What is the purpose of meta arguments
  count,for_each ,depends on ...
- Practice Count  creating 3 ec2 instances and create route53 records
- Practice for_each creating 3 ec2 instances and create route53 records
- using for and splat operator to print IPs of instances


- condition ? 1:0
- using condition creating resources only dev instance not prod
- conditional create example on s3 if value true it will create if false it won't create

- statefile
- local state,remote state & dynamic locking
- import
- modules

- terraform best practices
  - use modules
  - use remote backend and state locking
  - less use provisioners
  - don't hard code any sensitive data
  - for local resource names use _ for exposing names can be -
  - terraform plan before apply to find drift easily
  - scan terraform configuration files by using terraform tfsec 