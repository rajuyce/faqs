### ✅ Terraform Interview Questions & Answers

---

### ⚙️ Terraform Basics & Core Workflow

1. **What happens during `terraform plan` and `terraform apply`?**  
   - `terraform plan`: Shows the execution plan by comparing current state with desired config.  
   - `terraform apply`: Applies the planned changes to reach desired state.

2. **What are the different phases in Terraform?**  
   - Init → Validate → Plan → Apply → Destroy.

3. **What is the `.terraform` directory used for?**  
   - Stores module and provider binaries, backend config, and state lock info.

4. **What happens if you manually change a resource outside Terraform?**  
   - Terraform will detect drift during the next plan and may attempt to overwrite the change.

5. **How do you roll back infrastructure changes applied using Terraform?**  
   - Use version control to revert the code and apply again. No native rollback exists.

6. **How do you prevent Terraform drift?**  
   - Enforce IaC-only changes, regularly run `terraform plan`, use `terraform state` commands.

---

### 🧮 Variables & Expressions

7. **What are the different kinds of variables in Terraform?**  
   - Input variables, output values, local values, environment variables.

8. **What are local variables, and why are they useful?**  
   - Used to simplify expressions or avoid repeating complex logic.

9. **What are dynamic blocks in Terraform?**  
   - Allow conditional and looped generation of nested blocks.

10. **What is `null` in Terraform and where would you use it?**  
    - Used to omit arguments dynamically. If a value is `null`, Terraform acts as if it's not set.

11. **What is the difference between a map and an object in Terraform?**  
    - Map: key-value pairs with uniform value types.  
    - Object: structured key-value pairs with defined types per key.

---

### 📦 Modules & Reusability

12. **What are Terraform modules and 3 use cases for using them?**  
    - A module is a container for resources. Use cases:  
      - Reuse common infra patterns  
      - Logical grouping of related resources  
      - Enforcing organization-wide standards

13. **How do you pass outputs between modules?**  
    - Use `output` from source module and reference via `module.source_module.output_name`

14. **Which is best practice: having a Terraform module for S3 and lifecycle rules together or separately?**  
    - Separate modules for better separation of concerns and reuse.

---

### 🧪 Environments & Workspaces

15. **What are Terraform workspaces and when would you use them?**  
    - Isolated state per workspace; used for managing different environments like dev/stage/prod.

16. **How do you manage multiple environments using Terraform?**  
    - Options:  
      - Separate workspaces  
      - Directory structure per env  
      - Use of Terragrunt

---

### 📂 State Management & Backends

17. **What are the different kinds of backends in Terraform?**  
    - Local, remote (S3, Azure, GCS, etc.), enhanced (Terraform Cloud/Enterprise).

18. **What is a backend in Terraform, and how do you configure it securely?**  
    - Defines how state is stored. Use encryption (e.g., S3 + KMS) and restrict access with IAM.

19. **How do you manage state in a team environment?**  
    - Use remote backend with locking (e.g., S3 + DynamoDB), and versioning.

20. **How do you recover from a corrupted state file?**  
    - Use state file backups, state versioning (e.g., in S3), or manually fix using `terraform state`.

---

### 🔐 Credentials & Secrets Management

21. **How do you supply credentials in Terraform?**  
    - Environment variables, AWS CLI profile, shared credentials file, or Vault integration.

22. **How do you prevent secrets from being stored in the state file?**  
    - Avoid hardcoding secrets. Use data sources or external secrets managers.

23. **How does Terraform integrate with AWS Secrets Manager or Vault?**  
    - Use `data` blocks to read secrets dynamically at runtime.

---

### 🧰 Tools & Ecosystem

24. **What is Terragrunt?**  
    - Wrapper for Terraform that adds features like DRY code, dependency management, and automation.

25. **What is the difference between open-source Terraform and Terraform Enterprise?**  
    - Enterprise provides UI, team management, policies (Sentinel), audit logging, and private registry.

---

### 🧼 Resource Management & Refactoring

26. **I want to change the name of a resource and avoid creating a new one — how do I achieve this?**  
    - Use `terraform state mv` to rename the resource in the state before applying the config change.
