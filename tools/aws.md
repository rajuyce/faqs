### ☁️ AWS – Compute & Networking

#### EC2 & Networking
1. **Your EC2 instance is unreachable — how do you troubleshoot?**
   - Check security group rules, NACLs, and route tables.
   - Confirm instance state and system status checks.
   - Verify the correct subnet with a public IP or NAT for private subnets.
   - Validate DNS settings and SSH key.

2. **How do you design a highly available EC2 setup?**
   - Use Auto Scaling Groups (ASG) across multiple Availability Zones (AZs).
   - Use Elastic Load Balancer (ELB) to distribute traffic.
   - Configure health checks for automatic failover.

3. **Your EC2 instance is running but the application is not accessible — what do you check?**
   - Verify that the application/service is running.
   - Check the instance's security group and NACLs.
   - Ensure the correct listener/port is open.
   - Validate route tables and DNS resolution.

4. **How do you automate EC2 scaling?**
   - Use Auto Scaling Groups with scaling policies (CPU, memory, custom CloudWatch metrics).
   - Use scheduled scaling for predictable workloads.

5. **How do you recover a terminated or failed EC2 instance with minimal downtime?**
   - Use AMIs and EBS snapshots for quick recreation.
   - Use Auto Scaling to replace unhealthy instances automatically.

6. **How do you automate patching of EC2 instances?**
   - Use AWS Systems Manager Patch Manager with maintenance windows.

8. **What kind of services require a VPC in AWS?**
   - EC2, RDS, ECS (EC2 launch type), EKS, ElastiCache, Redshift, and more.

9. **How do you securely connect multiple VPCs across regions?**
   - Use AWS Transit Gateway with inter-region peering.
   - Use VPC peering or Site-to-Site VPN.

#### IAM & Roles
10. **Can you attach multiple IAM roles to EC2 or Lambda?**
    - No. Only one IAM role can be attached per EC2 instance or Lambda function.

11. **What IAM permissions does a Lambda function require?**
    - Basic execution role: `AWSLambdaBasicExecutionRole`.
    - Additional permissions based on services it interacts with (e.g., S3, DynamoDB).

12. **What permissions does a Lambda need for basic execution?**
    - CloudWatch Logs permissions for logging.

---

### 🌐 AWS – Storage & CDN

#### S3

2. **How do you make sure S3 data is not publicly accessible?**
   - Enable S3 Block Public Access at bucket/account level.
   - Review and restrict bucket policies and ACLs.

3. **How do you optimize S3 storage costs for rarely accessed data?**
   - Use storage classes like S3 Infrequent Access (IA), Glacier, or Intelligent-Tiering.

4. **What is an S3 Resource Policy and how does it work?**
   - A resource-based policy attached to a bucket to define access permissions for principals.

5. **An app suddenly loses access to S3. How do you debug IAM roles and policies?**
   - Check the IAM policy simulator.
   - Validate trust relationships and resource policies.

6. **How do you serve a static website with HTTPS using S3?**
   - Host site in S3, use CloudFront with a custom domain and ACM-issued SSL certificate.

---

### 🔐 AWS – Security & Compliance

#### IAM & Secrets
1. **How do you securely store and manage secrets in AWS?**
   - Use AWS Secrets Manager or Systems Manager Parameter Store (with encryption).

2. **How do you enforce MFA for IAM users?**
   - Set up MFA device and enforce via IAM policy conditions (`aws:MultiFactorAuthPresent`).

3. **What is a Service Control Policy (SCP) in AWS Organizations?**
   - Policy type that sets permission boundaries for accounts in an organization.

#### Network Security
4. **What are the different types of network security services in AWS?**
   - Security Groups, NACLs, AWS WAF, AWS Shield, Network Firewall, PrivateLink.

5. **What are network firewalls in AWS and how do you use them?**
   - AWS Network Firewall is a managed service to deploy stateful inspection rules at the VPC level.

6. **How do you prevent a Lambda function from accessing the internet?**
   - Deploy it in a private subnet without a NAT Gateway or Internet Gateway.

#### Cost & Access Monitoring
7. **Your AWS bill suddenly spikes. How do you investigate and control it?**
   - Use AWS Cost Explorer and Budgets.
   - Enable detailed billing reports and check CloudWatch for resource usage.
   - Set up billing alerts.

---

### 📡 AWS – Connectivity & Access
1. **What is a VPC Endpoint and in what cases do you use it?**
   - Enables private connection to AWS services without internet. Used for secure S3, DynamoDB, etc., access from VPC.

---

### 📊 AWS – Monitoring & Cost Optimization
1. **How do you monitor EC2 or Lambda performance and set alerts?**
   - Use CloudWatch metrics and alarms.
   - For EC2: CPU, memory, disk via CloudWatch Agent.
   - For Lambda: Duration, invocations, errors, throttles.

2. **What are filters in CloudWatch?**
   - Used in metric filters to extract values from log streams for custom metrics.

3. **How do you optimize costs for idle or dev environments?**
   - Use auto-shutdown scripts.
   - Leverage Spot Instances.
   - Use budgets and enforce policies with AWS Organizations.

---

### 🌀 AWS – Deployment & Automation
1. **How do you implement blue-green deployments in AWS?**
   - Use CodeDeploy with EC2, Lambda, or ECS.
   - Use weighted routing with Route 53 or ALB.

2. **How do you set up infrastructure as code with Terraform or CloudFormation?**
   - Write .tf (Terraform) or .yaml/.json (CloudFormation) templates.
   - Use modules/stacks, and apply changes via CI/CD pipelines.

3. **What is Amazon EventBridge and how is it used in automation?**
   - Event bus service for routing events. Used to trigger Lambda, Step Functions, etc.

4. **What are AWS Step Functions and when would you use them?**
   - Serverless workflows to sequence Lambda executions with retries, branching, and error handling.

5. **What is a Lambda function layer?**
   - A distribution mechanism for libraries, custom runtimes, and dependencies used across functions.

6. **What are the different phases in a Lambda function lifecycle?**
   - Initialization, invocation, and shutdown.

---

###  AWS – AI & ML Services
1. **What generative AI tools are available in AWS (e.g., Bedrock, SageMaker)?**
   - Bedrock for foundation models from Anthropic, Meta, etc.
   - SageMaker for custom ML models and pipelines.

2. **How do you integrate AI services with Lambda or Step Functions?**
   - Call Bedrock/SageMaker via SDKs inside Lambda.
   - Orchestrate using Step Functions for workflows.

---

### 🏗️ AWS – Landing Zone
1. **What is AWS Landing Zone and what are its key components?**
   - Pre-configured baseline with multi-account architecture, guardrails, logging, and security controls.

2. **What are the best practices for setting up a multi-account architecture using Landing Zone?**
   - Separate accounts by workload (e.g., security, shared services, dev/prod).
   - Enforce SCPs, and centralize logging and monitoring.

3. **How do you implement guardrails in an AWS Landing Zone?**
   - Use SCPs, AWS Config rules, and AWS Config conformance packs.

4. **How do you customize an AWS Landing Zone to match organization-specific compliance needs?**
   - Modify AWS Control Tower templates, custom pipelines, and SCPs.

5. **How do you integrate third-party tools in an AWS Landing Zone setup?**
   - Use Account Vending Machine extensions or pipelines.

6. **What’s the difference between a custom Landing Zone and AWS Control Tower?**
   - Control Tower provides a managed setup.
   - Custom LZ provides more flexibility with manual deployment.

---

### 🧭 AWS – Control Tower
1. **What is AWS Control Tower and how is it different from manually setting up a Landing Zone?**
   - Automated setup of multi-account environments with guardrails and baselines.

2. **How do you set up account baselines using Control Tower?**
   - Use Account Factory and Landing Zone templates with pre-configured settings.

3. **How does Control Tower enforce preventive and detective controls?**
   - SCPs (preventive) and AWS Config rules (detective).

4. **What are the key benefits of using AWS Control Tower for multi-account governance?**
   - Speed, compliance, central logging, automation, consistency.

5. **What lifecycle events can you automate using Control Tower?**
   - Account provisioning, guardrail updates, logging configurations.

6. **What are the common limitations or challenges with AWS Control Tower?**
   - Limited customization, regional restrictions, and dependency on supported services.

---

### 🧰 AWS – Account Factory
1. **What is AWS Account Factory and how does it work within Control Tower?**
   - Automated account provisioning system within Control Tower.

2. **How do you create and provision new accounts using Account Factory?**
   - Use AWS Service Catalog with input parameters for OU, VPC, email, etc.

3. **How do you enforce standard security baselines using Account Factory?**
   - Baselines are applied automatically via the Control Tower.

4. **How do you customize networking and IAM roles during account provisioning in Account Factory?**
   - Modify Account Factory templates and use customization pipelines.

5. **How do you handle updates or changes to existing accounts created via Account Factory?**
   - Use update pipelines, SCPs, and reapply blueprints via Control Tower.
