### Terraform provisioners. 
Provisioners are used to execute scripts or commands on a local or remote machine after resourceis created.
Types of Provisioners:
1. File
    Used to upload a file a from local to remote instance
2. remote-exec
    Runs commands/scripts on the remote resource  via SSH or WinRM
    Used to install packages, modify files, run configuration tools (like Ansible, Chef).

3. Local-exec
    Runs commands locally (on the machine running terraform apply).

### Terraform statefile

Terraform uses a statefile to track the real infrastructure it manages.
Why it is needed?
    Keep track of what has been created and what's need to change
    Helps `Terraform plan` and `Terraform apply` know the difference between your.tf code and what's deployed.
    Without state, Terraform wouldn't know what resources it manages.
    It maps your Terraform configuration to real-world resources (e.g., AWS, Azure, GCP).

there is a disadvantage of statefile being stored in local as it contains sensitive data like:
    passwords, secrets, Accesskeys

solution is remote backend;
A remote backend is where Terraform stores your state file `Terrafrom.tfstate` remotely instead of saving it locally on you machine. 

In Aws, store the state file in an S3 bucket
    locks the state using using DynamoDB
    Encrypt state using server-side encrytption.

### Docker image vs container
    Docker Image is a standarized package of your application that includes all the files, binaries, libraries, and configuration to run a container

    Container is actually running instance of an image, where the application runing.

### If we have created 3 instances using terraform script and the instance names are mentioned as a list
Suppose we removed 2nd instance name from the list and applied script again then what will happen to the already 3 instances created before.

It depends on how you create the instances whether using `count` or `for_each`. 

“If we use `count` and remove an item from the list, Terraform reindexes the resources. This causes existing instances to be destroyed and new ones created even if the instance name hasn’t changed.

To avoid this, I prefer using `for_each` which uses the instance name as a key — that way, only the specific removed instance gets deleted, and others remain untouched.”

### In Terraform, what is the purpose of init, plan, and apply commands?
`Terraform inti`- Initilizes the working directory by downloading necessary plugins and setting up the backend
`Terraform plan`- Shows proposed changes, whata will be created, changed, destroyed
`Terraform apply`- actually provisions the resources as described in the plan.

### What happens if the Terraform state file is accidentally deleted?
If `Terraform State` file is accidentally deleted, it loss the track of the existing infrastructure.
any subsequent apply, plan commands will recreate all the resources as new, causing duplication or failures.

The first step is to check for the backup- either in `.terraform/`, S3 versioning, or Terraform Cloud state history. 
Use terraform import (Rebuild State)
If no backup is available, we can use terraform import to rebuild the state resource

### What is the purpose of creating S3 bucket policies?
The purpose of creating policies is to control the access to S3 bucket and its contents. It defines who (principals) can do what (actions) on which resources, and under what conditions.

### How do you maintain the lifecycle of an S3 bucket?
To maintain the lifecycle of an S3 bucket, I configure lifecycle policies that automate object transitions between storage classes and delete data after a defined period

This helps manage costs and data retention — for example, I might move logs to Glacier after 30 days and delete them after 1 year. I typically define these rules using Terraform or the AWS Console, and align them with business compliance requirements.”

### Can u pls write terraform file to provision the Ec2 instance in a public subnet in a VPC?

### write sample terraform resource file

### What is Terraform and how do you use terraform in your project and what all resources have you provisioned 

### Backend.tf file is showing in repo but it is not showing in storage account, what may be the issue
Could be any reason:
1.Terraform is not initialized
2.State is still local not remote 
3.Incorect backend configuration
4. Authentication or Permission Issue

### What are the provisioners available in Terraform and can you explain the use cases?
    local-exec: Run local commands
    remote-exec: Run commands on remote resource
    file: Copy files to remote resource
### I have created an EC2 instance through Terraform. I don't have a backup of the Terraform state file, it is not in the remote state and locally not available. Now when I do apply, what can I do?

If the Terraform state file is lost and there's no remote or local copy, Terraform treats the infrastructure as non-existent. So if you run terraform apply, it will attempt to create everything from scratch, which can lead to:

    Resource duplication (e.g., new EC2 instance)
    Drift between actual infrastructure and declared config
In one project, a teammate deleted the local .tfstate and forgot to configure remote S3 state. We recovered the infrastructure by importing all critical resources (EC2, SG, EIP, IAM) and re-established state tracking — without recreating anything.

### Write a Terraform code to create multiple S3 buckets - notAnswered

### how are you managing the conflict? State file conflicts
    To manage Terraform state file conflicts:
        I use remote backends with state locking
        Follow a single source of truth principle
        Avoid parallel applies, and integrate Terraform with CI/CD pipelines responsibly

### what is difference between content and tuple in terraform?
1. content — Used in Dynamic Blocks
The content block is used inside a dynamic block to define what to render for each item in a loop.

2. tuple — A Type System Term
A tuple is a sequence of values with different types and fixed length — similar to a list, but each element may have a different type.

### what is diffrence between list and string in terraform?
1. string – A Single Text Value
A string is a single sequence of characters
2. list – An Ordered Collection of Values
A list holds multiple values in a specific order
Enclosed in square brackets []

### Terraform has errors while provisioning infrastructure. How to do investigate those? Basically how do you validate the terraform file
1. Validate the Configuration
 ```bash
terraform validate
```
✅ Checks for syntax correctness and basic internal consistency (e.g., missing arguments, wrong blocks, type mismatches).
 2. Format for Readability
 ```bash
 terraform fmt -recursive
```
Ensures your code follows standard formatting, which helps identify misplaced or nested blocks easily.

3. Run a Terraform Plan
```bash 
terraform plan
```
✅ This simulates changes and often surfaces runtime errors,
4. Check for Specific Error Types

This structured approach helps in quickly identifying and resolving Terraform provisioning errors.

### what is provider?
In Terraform, a provider is a plugin that allows Terraform to interact with APIs of external platforms or services like AWS, Azure, Google Cloud, Kubernetes, GitHub, and many more.
It acts as a bridge between Terraform and the infrastructure you're managing.

### Do you store the data in statefile locally or remotely. What is the block you use while storing the statefile.
I store the statefile remotely, I use  `terraform` and `backend` block to store in s3.

### what is terraform module.
A Terraform module is a reusable, self-contained package of Terraform configurations that manages a specific set of resources. Modules help:

    Organize code
    Reuse infrastructure
    Maintain consistency
    Share infrastructure patterns
    Encapsulate complexity
    Enable versioning

5.how to manage multiple env management in terrfaform?
I manage multiple environments in Terraform using a combination of workspaces, directory structure, and variable overrides to ensure clean separation and controlled deployment across dev, staging, and production.
 1. Directory Structure per Environment (Most Common)
```bash
terraform/
├── modules/
│   └── network/
├── envs/
│   ├── dev/
│   │   ├── main.tf
│   │   ├── vars.tf
│   │   └── terraform.tfvars
│   ├── stage/
│   └── prod/
```
✅ Benefits:
    Full isolation per environment
    Environment-specific configs, variables, and backends

2. Terraform Workspaces (Logical Separation)
```bash 
terraform workspace new dev
terraform workspace new prod
```
### what is the command for auto approval in terraform 
 ```bash
 terraform apply -auto-approve
```
What It Does:
Applies the Terraform plan without prompting you to type yes
Useful in automation, CI/CD pipelines, and scripting where manual approval is not possible

### What is the difference between a map of objects in Terraform and how can you write an example?-notAnswered
 How would you migrate a Terraform backend from local to a remote backend like S3 with DynamoDB locking?
 How do you update the statefile from local to S3 bucket,what will you do if it gets lost.
  Terraform scripts for creating AWS services Jenkinsfile EKS and On Prem Kubernetes cluster upgrade steps.

  What are best practices to be followed on terraform,

  You created couple of resources using Terraform, how will you make sure that resources are not modified through UI, how will you automate this check