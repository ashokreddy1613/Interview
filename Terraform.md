# Terraform Interview Questions and Answers

## 📁 Core Concepts

### 1. What is Terraform and how do you use it in your project?
Terraform is an Infrastructure as Code (IaC) tool that allows you to build, change, and version infrastructure safely and efficiently. It uses a declarative configuration language called HCL (HashiCorp Configuration Language).

Key uses in a project:
- Define infrastructure in code
- Version control infrastructure changes
- Automate infrastructure deployment
- Ensure consistency across environments
- Manage multiple cloud providers
- Enable collaboration through code review
- Implement infrastructure testing
- Support disaster recovery

### 2. What is Terraform state file?
The Terraform state file is a JSON file that maps real-world resources to your configuration. It serves several critical purposes:

- Maps resources to configuration
- Tracks resource dependencies
- Stores resource metadata
- Improves performance for large infrastructures
- Enables resource tracking across team members
- Facilitates resource updates and deletions

### 3. What happens if the Terraform state file is accidentally deleted?
If the Terraform state file is deleted:
1. Terraform loses track of managed resources
2. Cannot determine existing resources in the cloud
3. May attempt to recreate all resources
4. Could lead to duplicate resources
5. May cause orphaned resources
6. Requires manual intervention to recover

### 4. How do you manage Terraform state file?
Best practices for state management:

1. Use remote state storage:
```hcl
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "path/to/state/file"
    region = "us-west-2"
    dynamodb_table = "terraform-lock"
  }
}
```

2. Enable state locking
3. Implement state file encryption
4. Use state file versioning
5. Implement backup strategies
6. Use workspaces for environment separation

### 5. How do you update the statefile from local to S3 bucket?
To migrate state to S3:

1. Configure S3 backend:
```hcl
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "path/to/state/file"
    region = "us-west-2"
    dynamodb_table = "terraform-lock"
  }
}
```

2. Initialize the new backend:
```bash
terraform init -migrate-state
```

If state is lost:
1. Use `terraform import` to reimport resources
2. Recreate state from existing infrastructure
3. Use backup if available
4. Document and recreate if necessary

### 6. What is the lock file in Terraform?
The lock file (`.terraform.lock.hcl`) is used to:
- Lock provider versions
- Ensure consistent provider versions across team
- Prevent version conflicts
- Track provider checksums
- Enable reproducible builds

### 7. How do you store the data in the state file locally or remotely?
State can be stored in two ways:

1. Locally (default):
```hcl
terraform {
  backend "local" {
    path = "relative/path/to/terraform.tfstate"
  }
}
```

2. Remotely (S3 example):
```hcl
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "path/to/state/file"
    region = "us-west-2"
  }
}
```

### 8. What block do you use while storing the state file?
Use the `terraform` block with `backend` configuration:
```hcl
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "path/to/state/file"
    region = "us-west-2"
  }
}
```

## ⚙️ Commands and Operations

### 1. What is the purpose of terraform init, plan, and apply?
- `terraform init`: 
  - Initializes a working directory
  - Downloads providers and modules
  - Sets up backend configuration
  - Prepares for Terraform operations

- `terraform plan`:
  - Creates an execution plan
  - Shows what actions will be taken
  - Validates configuration
  - Checks for errors

- `terraform apply`:
  - Applies the changes
  - Creates/updates infrastructure
  - Updates state file
  - Handles resource dependencies

### 2. What is the command for auto approval in Terraform?
```bash
terraform apply -auto-approve
```

### 3. How do you validate a Terraform file if provisioning fails?
1. Use `terraform validate` to check syntax
2. Use `terraform plan` to preview changes
3. Check logs and error messages
4. Use `terraform state list` to verify state
5. Use `terraform show` to inspect current state

### 4. What happens to the Terraform state file if someone deletes resources manually from cloud?
The state file becomes out of sync. To fix:
1. Run `terraform plan` to see the drift
2. Use `terraform import` to reimport the resource
3. Or use `terraform state rm` to remove the resource from state
4. Then run `terraform apply` to reconcile the state

### 5. Suppose I have given one command in null_resource, it should run every time. What is the behavior?
Use the `triggers` block with a timestamp:
```hcl
resource "null_resource" "example" {
  triggers = {
    always_run = timestamp()
  }
  
  provisioner "local-exec" {
    command = "your-command-here"
  }
}
```

## 🧱 Resources and Provisioning

### 1. Have you written any Terraform code to build a VM?
Yes, here's an example for AWS EC2:
```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  
  tags = {
    Name = "example-instance"
  }
}
```

### 2. Write a Terraform code to create multiple S3 buckets
```hcl
variable "bucket_names" {
  type = list(string)
  default = ["bucket1", "bucket2", "bucket3"]
}

resource "aws_s3_bucket" "buckets" {
  for_each = toset(var.bucket_names)
  
  bucket = each.value
  
  tags = {
    Name = each.value
  }
}
```

### 3. Write a sample Terraform resource file
```hcl
provider "aws" {
  region = "us-west-2"
}

resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
  
  tags = {
    Name = "main"
  }
}

resource "aws_subnet" "main" {
  vpc_id     = aws_vpc.main.id
  cidr_block = "10.0.1.0/24"
  
  tags = {
    Name = "main"
  }
}
```

### 4. Write Terraform code to provision an EC2 instance with a security group allowing only SSH access
```hcl
resource "aws_security_group" "ssh" {
  name        = "ssh-access"
  description = "Allow SSH access"
  vpc_id      = aws_vpc.main.id

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  vpc_security_group_ids = [aws_security_group.ssh.id]

  tags = {
    Name = "example-instance"
  }
}
```

### 5. Write a Terraform code to create VPC, subnet, EC2, S3 bucket
```hcl
# VPC
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
  
  tags = {
    Name = "main"
  }
}

# Subnet
resource "aws_subnet" "main" {
  vpc_id     = aws_vpc.main.id
  cidr_block = "10.0.1.0/24"
  
  tags = {
    Name = "main"
  }
}

# EC2 Instance
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  subnet_id     = aws_subnet.main.id
  
  tags = {
    Name = "example-instance"
  }
}

# S3 Bucket
resource "aws_s3_bucket" "example" {
  bucket = "my-example-bucket"
  
  tags = {
    Name = "example-bucket"
  }
}
```

### 6. Can you write a Terraform file to provision an EC2 instance in a public subnet?
```hcl
# VPC
resource "aws_vpc" "main" {
  cidr_block = "10.0.0.0/16"
  
  tags = {
    Name = "main"
  }
}

# Internet Gateway
resource "aws_internet_gateway" "main" {
  vpc_id = aws_vpc.main.id
  
  tags = {
    Name = "main"
  }
}

# Public Subnet
resource "aws_subnet" "public" {
  vpc_id                  = aws_vpc.main.id
  cidr_block              = "10.0.1.0/24"
  map_public_ip_on_launch = true
  
  tags = {
    Name = "public"
  }
}

# Route Table
resource "aws_route_table" "public" {
  vpc_id = aws_vpc.main.id

  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.main.id
  }
  
  tags = {
    Name = "public"
  }
}

# Route Table Association
resource "aws_route_table_association" "public" {
  subnet_id      = aws_subnet.public.id
  route_table_id = aws_route_table.public.id
}

# EC2 Instance
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  subnet_id     = aws_subnet.public.id
  
  tags = {
    Name = "example-instance"
  }
}
```

## 🔗 Modules and Environments

### 1. What is a Terraform module?
A Terraform module is a reusable, self-contained package of Terraform configurations that manages a specific set of resources. Modules help:
- Organize code
- Reuse infrastructure
- Maintain consistency
- Share infrastructure patterns
- Encapsulate complexity
- Enable versioning

### 2. How do you manage multiple environment configurations in Terraform?
1. Use workspaces
2. Use separate state files
3. Use variable files
4. Use modules with environment-specific variables
5. Use remote state with different keys

Example:
```hcl
# terraform.tfvars for dev
environment = "dev"
instance_type = "t2.micro"

# terraform.tfvars for prod
environment = "prod"
instance_type = "t3.large"
```

### 3. Best practices to structure repos and pipelines in a large DevOps project?
1. Use a modular structure
2. Separate environments
3. Use remote state
4. Implement CI/CD
5. Use version control
6. Implement testing
7. Use consistent naming
8. Document everything

### 4. How do you scale a Terraform pipeline that takes 25+ mins?
1. Use parallel execution
2. Implement caching
3. Use remote state
4. Optimize provider usage
5. Use targeted applies
6. Implement state locking
7. Use workspaces
8. Optimize resource dependencies

## 🧩 Provisioners and Logic

### 1. What are the provisioners available in Terraform and can you explain the use cases?
1. `local-exec`: Run local commands
2. `remote-exec`: Run commands on remote resource
3. `file`: Copy files to remote resource
4. `chef`: Chef provisioner
5. `puppet`: Puppet provisioner
6. `salt-masterless`: Salt provisioner

### 2. What is the difference between a map and object in Terraform?
- Map: Key-value pairs where all values must be of the same type
- Object: Key-value pairs where values can be of different types

Example:
```hcl
# Map
variable "map_example" {
  type = map(string)
  default = {
    key1 = "value1"
    key2 = "value2"
  }
}

# Object
variable "object_example" {
  type = object({
    name = string
    age  = number
  })
  default = {
    name = "John"
    age  = 30
  }
}
```

### 3. What is the difference between list and string in Terraform?
- List: Ordered collection of values of the same type
- String: Single text value

Example:
```hcl
# List
variable "list_example" {
  type = list(string)
  default = ["item1", "item2", "item3"]
}

# String
variable "string_example" {
  type = string
  default = "single value"
}
```

## 🛡️ Security & IAM Integration

### 1. How do you provide AWS access to new users using Terraform?
```hcl
resource "aws_iam_user" "new_user" {
  name = "new-user"
}

resource "aws_iam_user_policy" "user_policy" {
  name = "user-policy"
  user = aws_iam_user.new_user.name

  policy = jsonencode({
    Version = "2012-10-17"
    Statement = [
      {
        Action = [
          "s3:ListBucket",
          "s3:GetObject"
        ]
        Effect   = "Allow"
        Resource = "*"
      }
    ]
  })
}
```

### 2. How do you enforce Azure Policies using Terraform at scale?
1. Use Azure Policy assignments
2. Use policy initiatives
3. Use policy definitions
4. Use policy exemptions
5. Use policy remediation

Example:
```hcl
resource "azurerm_policy_assignment" "example" {
  name                 = "example-policy"
  scope                = azurerm_resource_group.example.id
  policy_definition_id = azurerm_policy_definition.example.id
  description          = "Policy Assignment created via Terraform"
  display_name         = "Example Policy Assignment"
}
```

## 🎯 Advanced Scenarios and Best Practices

### 1. Zero-Downtime Deployment
**Scenario:** Update EC2 instance's AMI ID without downtime.

**Solution:**
```hcl
resource "aws_instance" "example" {
  ami           = var.ami_id
  instance_type = "t3.micro"

  lifecycle {
    create_before_destroy = true
  }
}
```

**Key Points:**
- Use `create_before_destroy` lifecycle rule
- Ensures new instance is created before old one is deleted
- Alternative: Use Auto Scaling Group with rolling updates

### 2. Cross-Region Infrastructure
**Scenario:** Deploy S3 bucket in us-east-1 and EC2 in ap-south-1.

**Solution:**
```hcl
provider "aws" {
  alias  = "us-east"
  region = "us-east-1"
}

provider "aws" {
  alias  = "ap-south"
  region = "ap-south-1"
}

resource "aws_s3_bucket" "example" {
  provider = aws.us-east
  bucket   = "my-bucket-us-east"
}

resource "aws_instance" "example" {
  provider      = aws.ap-south
  ami           = "ami-123456"
  instance_type = "t2.micro"
}
```

### 3. Handling Infrastructure Drift
**Scenario:** Manual changes made to Terraform-managed infrastructure.

**Solution:**
1. Detect drift:
```bash
terraform plan -refresh-only
```

2. Re-import modified resources:
```bash
terraform import aws_instance.example i-1234567890abcdef0
```

3. Apply to restore configuration:
```bash
terraform apply
```

### 4. Security Policy Enforcement
**Scenario:** Restrict EC2 instance types to control costs.

**Solution:**
```hcl
variable "instance_type" {
  description = "AWS EC2 instance type"
  type        = string

  validation {
    condition     = contains(["t2.micro"], var.instance_type)
    error_message = "Only t2.micro instance type is allowed."
  }
}
```

### 5. Blue-Green Deployment
**Scenario:** Implement blue-green deployment for Auto Scaling Group.

**Solution:**
```hcl
resource "aws_launch_template" "blue" {
  name = "blue-template"
  image_id = var.ami_blue
}

resource "aws_launch_template" "green" {
  name = "green-template"
  image_id = var.ami_green
}

resource "aws_autoscaling_group" "blue" {
  launch_template {
    id = aws_launch_template.blue.id
  }
}

resource "aws_autoscaling_group" "green" {
  launch_template {
    id = aws_launch_template.green.id
  }
}

resource "aws_lb_listener_rule" "switch" {
  listener_arn = aws_lb_listener.http.arn
  priority     = 100
  conditions {
    field  = "path-pattern"
    values = ["*"]
  }
  actions {
    type             = "forward"
    target_group_arn = aws_lb_target_group.green.arn
  }
}
```

### 6. API Rate Limit Handling
**Scenario:** Creating 100+ AWS resources with API rate limits.

**Solution:**
1. Configure retry settings:
```hcl
provider "aws" {
  region = "us-east-1"
  max_retries = 5
}
```

2. Stagger resource creation:
```hcl
resource "aws_instance" "one" {
  ami = "ami-123456"
}

resource "aws_instance" "two" {
  ami       = "ami-123456"
  depends_on = [aws_instance.one]
}
```

### 7. Resource Protection
**Scenario:** Prevent accidental deletion of production RDS.

**Solution:**
```hcl
resource "aws_db_instance" "production_db" {
  identifier = "prod-db"
  engine     = "mysql"
  instance_class = "db.t3.large"

  lifecycle {
    prevent_destroy = true
  }
}
```

### 8. Multi-Account Management
**Scenario:** Managing multiple AWS accounts (Dev, QA, Prod).

**Solution:**
```hcl
provider "aws" {
  region = "us-east-1"
  alias  = terraform.workspace
}

resource "aws_s3_bucket" "example" {
  bucket = "my-bucket-${terraform.workspace}"
}
```

### 9. State Lock Resolution
**Scenario:** Resolving locked Terraform state.

**Solution:**
1. Force unlock:
```bash
terraform force-unlock <LOCK_ID>
```

2. Verify state:
```bash
terraform state list
terraform state show
```

### 10. Deployment Rollback
**Scenario:** Rolling back failed Terraform deployment.

**Solution:**
1. Restore previous state:
```bash
terraform state pull > backup.tfstate
terraform state push backup.tfstate
```

2. Revert code and reapply:
```bash
terraform apply
```

### 11. Dynamic Resource Scaling
**Scenario:** Different EC2 instance types per environment.

**Solution:**
```hcl
variable "instance_type_map" {
  type = map(string)
  default = {
    dev  = "t2.micro"
    prod = "t3.large"
  }
}

resource "aws_instance" "example" {
  ami           = "ami-123456"
  instance_type = var.instance_type_map[var.environment]
}
```

### 12. Secret Management
**Scenario:** Securely managing AWS credentials.

**Solution:**
```hcl
data "aws_secretsmanager_secret_version" "db_creds" {
  secret_id = "my-db-creds"
}

resource "aws_db_instance" "db" {
  username = jsondecode(data.aws_secretsmanager_secret_version.db_creds.secret_string)["username"]
  password = jsondecode(data.aws_secretsmanager_secret_version.db_creds.secret_string)["password"]
}
```

### 13. State Management
**Scenario:** Managing large Terraform state files.

**Solution:**
```hcl
terraform {
  backend "s3" {
    bucket         = "my-terraform-state"
    key            = "prod/terraform.tfstate"
    region         = "us-east-1"
    dynamodb_table = "terraform-lock"
  }
}
```

### 14. EKS Auto-Scaling
**Scenario:** Auto-scaling EKS worker nodes.

**Solution:**
```hcl
resource "aws_autoscaling_group" "eks_nodes" {
  min_size         = 2
  max_size         = 10
  desired_capacity = 3

  tag {
    key                 = "kubernetes.io/cluster/my-cluster"
    value               = "owned"
    propagate_at_launch = true
  }
}
```

### 15. Resource Recreation Prevention
**Scenario:** Preventing EC2 instance recreation.

**Solution:**
```hcl
resource "aws_instance" "example" {
  ami           = "ami-123456"
  instance_type = "t2.micro"

  lifecycle {
    ignore_changes = [instance_type]
  }
}
```

**Manual Update Process:**
1. Update instance outside Terraform
2. Import changes:
```bash
terraform import aws_instance.example i-1234567890abcdef
```

### 1. Suppose you run "terraform apply" and accidentally delete resources — how would you recover?
1. Stop Immediately
Do not run another terraform apply or terraform destroy. Doing so can overwrite valuable state or destroy remaining infrastructure.

2. Identify What Was Deleted
Review the Terraform output: it logs all resource deletions
```bash
   terraform show
   terraform plan
```
- Check terraform plan output if it’s still visible in your terminal history.
- Look at the .tf files to understand what resources are missing or changed.


2. Recover from State File Backup 

Most backends like S3 automatically version the terraform.tfstate file

3. Restore Previous State File
- If you’re using a remote backend (like S3), use the versioned terraform.tfstate to roll back:
- Download a good version and Replace current state
 
4. Verify Recovery
run `terraform show` to verify

### How do you troubleshoot a failing Terraform plan?

1. Read the Exact Error Message
✅ Terraform is very descriptive. Common failures include:
- Syntax errors
- Provider version mismatch
- Missing variables
- Invalid resource references
- API errors from the cloud provider (e.g., AWS, Azure)

2. Validate the Configuration
- run `terraform validate`
- Catches syntax errors, typos, and bad expressions
- Useful after recent module or variable changes

✅ Run this before plan to detect structural problems

3. Ensure All Required Variables Are Provided
- Terraform will fail if required inputs are missing

4. Check Cloud Provider Authentication & Permissions
- Many plan failures are due to missing or expired credentials.
- `aws sts get-caller-identity`

5. Check Backend Configuration (if using remote state)
If using S3, GCS, or Terraform Cloud:
- Ensure backend is configured correctly in backend block
- Make sure remote state locking is working (e.g., DynamoDB in AWS)
6. Refresh the State
- Sometimes stale state can cause plan failures:
- run `terraform refresh`
✅ This syncs Terraform’s local state with the actual infrastructure

### Your Terraform state file got corrupted — what would be your action plan?
1. check for backup - `terraform.tfstate.backup`
replace the corrupted state with the backup 
```cp terraform.tfstate.backup terraform.tfstate
```
then run `terraform show`

2. Recover from Remote Backend
Download a previous good version

3. Manually Patch the State File (advanced)
- If no backup is available and the damage is partial:
- Open the file with a JSON-aware editor
- Fix obvious formatting issues
- Validate with terraform show or terraform validate

4. If Nothing Works: Rebuild from Infrastructure
- Manually recreate the .tf definitions
- Use terraform import to reconstruct the state


### How would you safely manage sensitive variables like passwords and API keys in Terraform?

I manage sensitive variables in Terraform using secure input methods, encryption-aware backends,
and secret management systems like AWS Secrets Manager. I avoid hardcoding secrets in .tf files
or state whenever possible, and apply least-privilege access controls to limit exposure.
1. Mark Variables as Sensitive in variables.tf
```bash
  variable "db_password" {
    description = "RDS master password"
    type        = string
    sensitive   = true
  }
```
Prevents the password from being shown in CLI output or logs.

2. Do NOT Hardcode Secrets in .tf or .tfvars
3. Use Encrypted Remote Backends for State Storage
4. Use Secret Management Tools
Instead of managing raw secrets in Terraform, use external tools:
Keeps the actual secret in AWS-managed storage, not in .tfvars or tfstate.

###  Terraform execution is very slow with many modules — how would you
optimize it?

To speed up Terraform execution with many modules, I increase parallelism, skip refresh 
when safe, use targeted applies during dev, and split infrastructure into smaller root modules.
 I also reduce duplicate data sources and monitor slow modules using logs or plan profiling 
 tools."

### How would you perform drift detection between infrastructure and
Terraform code?

I perform drift detection by using Terraform's built-in `terraform plan` or `terraform plan -detailed-exitcode` to compare the actual infrastructure state with the desired configuration. In larger setups, I automate drift detection as part of CI/CD or GitOps pipelines and use tools like AWS Config or driftctl for continuous visibility.

### How would you handle versioning of Terraform modules across teams?

I follow a structured versioning strategy using Git tags and semantic versioning for our Terraform modules. Each module is developed and stored in a separate Git repository or in a central monorepo with clearly defined module folders.
When a module reaches a stable state, I tag it with a version like v1.0.0, and teams reference 
that version in their configurations using the ?ref= syntax or a private Terraform registry. This ensures consistency and prevents breaking changes.

### How would you automate Terraform plans and applies using CI/CD pipelines?
To automate Terraform plans and applies in CI/CD, I integrate Terraform into the pipeline stages using tools like Jenkins, GitHub Actions, or GitLab CI. I separate the plan and apply steps to enforce safe review and approvals. The pipeline handles formatting, validation, planning, and gated applies using versioned remote state and secure credentials.

### Backend.tf file is showing in repo but it is not showing in storage account, what may be the issueCould be any reason:
1. Terraform is not initialized
2. State is still local not remote 
3. Incorect backend configuration
4. Authentication or Permission Issue

### I have created an EC2 instance through Terraform. I don't have a backup of the Terraform state file, it is not in the remote state and locally not available. Now when I do apply, what can I do?

If the Terraform state file is lost and there's no remote or local copy, Terraform treats the infrastructure as non-existent. So if you run terraform apply, it will attempt to create everything from scratch, which can lead to:
- Resource duplication (e.g., new EC2 instance)
- Drift between actual infrastructure and declared config
In one project, a teammate deleted the local .tfstate and forgot to configure remote S3 state. We recovered the infrastructure by importing all critical resources (EC2, SG, EIP, IAM) and re-established state tracking — without recreating anything.

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


### how to manage multiple env management in terrfaform?
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
## What is the difference between for_each and for in Terraform?
for_each is used to create multiple copies of a resource or module, where each copy is uniquely identified by a key. It's applied at the resource level.

for, on the other hand, is an expression used to transform or filter collections like lists or maps inside locals, variables, or outputs.

For example, I might use for_each to create one S3 bucket per environment, and use for to uppercase all environment names before tagging.

## Diff between local and variable in Terraform?
variable is used to accept input from users or environments — like region, instance count, etc.

local is used for internal logic — to define constants or compute values from variables, like building a resource name or map

## You created couple of resources using Terraform, how will you make sure that resources are not modified through UI, how will you automate this check?
To prevent manual changes, I enforce least-privilege IAM access and restrict resource modification to Terraform pipelines.

For drift detection, I use terraform plan in CI/CD to automatically check for changes. If we're using Terraform Cloud, it offers built-in drift detection and alerting.

Optionally, we run scheduled terraform apply -refresh-only jobs to keep the state in sync and notify if any changes are detected

## What is terraform drift command
The terraform drift command allows you to detect and inspect drift in your infrastructure — i.e., changes made outside Terraform (like in a cloud console).

## Suppose there are 1000 of lines in terraform, as a period of time it grows, it becomes slow in future, how to approach this issue, please explain

1. Split Configuration Using Modules
2. Use Workspaces or Environments
Split environments like dev, stage, prod into separate state files.

Avoid managing everything in a single state.

3. Remote State Management with Backends

"In a large Terraform codebase, the key is modularization and state isolation.

I'd split the code using modules for components like network, compute, and IAM. For scaling environments, I'd use either directory-based structures or Terragrunt.

I'd move state to a remote backend like S3 or Terraform Cloud and use CI/CD pipelines to automate and parallelize plan and apply.

This way, we improve speed, reduce risk, and maintain clean, scalable infrastructure code."

## If someone deleted a resource in terraform how can you identify it and recover it. how to detect some one deleted in tf how to rectifty

If a resource managed by Terraform is deleted outside Terraform (e.g., manually from AWS/GCP/Azure console), you need to detect the deletion (drift) and recover it through Terraform.
run `terraform plan` to detect the deletion

Terraform is designed to be declarative — if a resource is missing but still present in your .tf files, simply run:
`terraform apply`
Terraform will recreate the deleted resource exactly as defined.
## How will you connect your terraform environment from aws and implement CI/CD.
In Jenkins, I set up Terraform to authenticate with AWS using stored credentials or IAM roles. I configure remote state in S3 with DynamoDB locking to avoid conflicts.
The pipeline runs terraform init, plan, and optionally apply on the main branch, ensuring that infrastructure changes are reviewed and version-controlled.
For production, I’d add manual approval, OIDC auth, and split apply into a gated stage."

## What is terraform work space and how are you managing it.
## terraform uses real time issue we faced…what are the error u faced in terraform recently
## terraform structure
