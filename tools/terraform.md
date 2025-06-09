# 📘 Terraform Interview Questions & Answers

## ⚙️ Terraform Basics & Core Workflow

### What happens during `terraform plan` and `terraform apply`?
- **`terraform plan`**: Compares the current infrastructure with the desired configuration and shows a preview of the changes that will be made.
  - *Example*: If you add a new S3 bucket in the configuration, `terraform plan` shows `+` for creating that bucket.
- **`terraform apply`**: Executes the plan and applies the changes to reach the desired state.

### What are the different phases in Terraform?
1. **Init**: Initializes backend and providers.
2. **Validate**: Checks configuration syntax and internal consistency.
3. **Plan**: Determines what actions are needed to achieve the desired state.
4. **Apply**: Makes changes to infrastructure.
5. **Destroy**: Tears down infrastructure managed by Terraform.

### What is the `.terraform` directory used for?
- Stores provider plugins, module caches, backend settings, and state lock information.

### What happens if you manually change a resource outside Terraform?
- Terraform detects the drift during the next `terraform plan` and shows that the current state differs from configuration. It may propose to overwrite manual changes.

### How do you roll back infrastructure changes applied using Terraform?
- There is no native rollback.
- Revert `.tf` configuration using version control and re-apply.
- In critical setups, use snapshots or resource backups manually.

### How do you prevent Terraform drift?
- Use Terraform exclusively for all infrastructure changes.
- Run `terraform plan` regularly.
- Implement CI pipelines with Terraform plan and apply stages.
- Use `terraform state` commands to inspect or fix drift.

## 🧮 Variables & Expressions

### What are the different kinds of variables in Terraform?
- **Input variables**: Used to pass dynamic values.
- **Output values**: Expose values after deployment.
- **Local values**: Used for simplified logic.
- **Environment variables**: Used to override input variables.

### What are local variables, and why are they useful?
- Local values simplify expressions and reduce repetition.
```hcl
locals {
  instance_type = "t3.micro"
}
```

### What are dynamic blocks in Terraform?
- Used to generate nested blocks conditionally or in loops.
```hcl
resource "aws_security_group" "example" {
  dynamic "ingress" {
    for_each = var.rules
    content {
      from_port = ingress.value.from
      to_port   = ingress.value.to
      protocol  = ingress.value.protocol
    }
  }
}
```

### What is `null` in Terraform and where would you use it?
- Represents an unset or optional value.
- If a value is `null`, the attribute is omitted in the resource.

### What is the difference between a map and an object in Terraform?
- **Map**: Key-value pairs with uniform value types.
- **Object**: Key-value pairs with explicitly defined types per key.
```hcl
variable "tags" {
  type = map(string)
}

variable "person" {
  type = object({
    name = string
    age  = number
  })
}
```

## 📦 Modules & Reusability

### What are Terraform modules and 3 use cases for using them?
- Modules are containers for multiple resources.
#### Use cases:
1. Reuse infrastructure patterns (e.g., VPC, IAM).
2. Logical grouping (e.g., RDS module).
3. Enforce organization-wide standards.

### How do you pass outputs between modules?
- Define output in the source module:
```hcl
output "vpc_id" {
  value = aws_vpc.main.id
}
```
- Reference it in the parent module:
```hcl
module.vpc.vpc_id
```

### Which is best practice: having a Terraform module for S3 and lifecycle rules together or separately?
- Prefer separate modules for better reusability, testing, and separation of concerns.

## 🧪 Environments & Workspaces

### What are Terraform workspaces and when would you use them?
- Workspaces allow isolated state files.
- Use for managing multiple environments (dev/staging/prod).
```bash
terraform workspace new dev
terraform workspace select prod
```

### How do you manage multiple environments using Terraform?
- Strategies:
  - Named workspaces
  - Directory per environment (e.g., `env/dev/main.tf`)
  - Terragrunt with folder hierarchy

## 📂 State Management & Backends

### What are the different kinds of backends in Terraform?
- **Local**: Stores state locally (default).
- **Remote**:
  - AWS S3 (with DynamoDB lock)
  - Azure Blob Storage
  - Google Cloud Storage
- **Enhanced**:
  - Terraform Cloud/Enterprise

### What is a backend in Terraform, and how do you configure it securely?
- Backend controls where state is stored.
- For secure S3 backend:
```hcl
backend "s3" {
  bucket         = "my-terraform-state"
  key            = "global/s3/terraform.tfstate"
  region         = "us-east-1"
  encrypt        = true
  dynamodb_table = "terraform-locks"
}
```
- Use IAM roles/policies to restrict access.

### How do you manage state in a team environment?
- Use remote backend with state locking (e.g., S3 + DynamoDB).
- Enable versioning.
- Avoid local state and manual updates.

### How do you recover from a corrupted state file?
- Recover from versioned backup (e.g., S3).
- Use `terraform state pull` and manual editing if necessary.
- `terraform import` can recreate missing resources into state.

## 🔐 Credentials & Secrets Management

### How do you supply credentials in Terraform?
- Via:
  - AWS environment variables
  - Shared credentials file (`~/.aws/credentials`)
  - AWS profiles
  - Vault integration

### How do you prevent secrets from being stored in the state file?
- Avoid inline secrets.
- Use data sources for fetching secrets at runtime.
- Use external secret managers (e.g., Vault, AWS Secrets Manager).

### How does Terraform integrate with AWS Secrets Manager or Vault?
```hcl
data "aws_secretsmanager_secret_version" "example" {
  secret_id = "my-secret"
}

resource "aws_instance" "example" {
  user_data = data.aws_secretsmanager_secret_version.example.secret_string
}
```

## 🧰 Tools & Ecosystem

### What is Terragrunt?
- A wrapper for Terraform that provides:
  - DRY configuration
  - Dependency management
  - Environment hierarchy
  - Automatic backend configuration

### What is the difference between open-source Terraform and Terraform Enterprise?
- **Terraform Open Source**:
  - CLI based
  - Community support
- **Terraform Enterprise**:
  - UI and API-based workflows
  - RBAC and team management
  - Sentinel policies for governance
  - Integrated state management and VCS support
