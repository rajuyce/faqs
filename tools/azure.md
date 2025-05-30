### ☁️ Azure – Compute & Networking

#### Virtual Machines & Networking
1. **Your Azure VM is unreachable — how do you troubleshoot?**
   - Check NSG rules, UDRs, and route tables.
   - Confirm VM status and Boot Diagnostics.
   - Verify subnet association and public IP configuration.
   - Validate DNS resolution and SSH/RDP settings.

2. **How do you design a highly available VM setup in Azure?**
   - Use Availability Sets or Availability Zones.
   - Use Azure Load Balancer or Application Gateway.
   - Configure autoscale with Virtual Machine Scale Sets (VMSS).

3. **VM is running but app is inaccessible — what do you check?**
   - Ensure the service is running inside the VM.
   - Check NSG, subnet, and firewall settings.
   - Validate DNS and Application Gateway configuration.

4. **How do you automate scaling of VMs in Azure?**
   - Use VMSS with autoscale rules based on metrics (CPU, custom metrics).
   - Use scheduled scaling profiles.

5. **How do you handle a failed or deallocated VM?**
   - Use VM backup or custom images to recreate.
   - Use VMSS for automatic instance replacement.

6. **How do you automate patching of Azure VMs?**
   - Use Azure Automation Update Management.

8. **Which services require a VNet in Azure?**
   - VMs, Azure Kubernetes Service (AKS), Azure SQL Managed Instance, Azure App Service (with VNet Integration), Azure Functions (Premium), etc.

9. **How do you securely connect VNets across regions?**
   - Use VNet Peering or Azure VPN Gateway with VNet-to-VNet connections.
   - Use Azure Virtual WAN for large-scale setups.

#### IAM & Roles
10. **Can you assign multiple Managed Identities to an Azure VM?**
    - No, only one system-assigned identity; multiple user-assigned identities are supported.

11. **What IAM permissions does a Function App require?**
    - Depends on services it accesses. Assign a managed identity and RBAC roles.

12. **What permissions are required for Azure Monitor logging?**
    - Read permissions on resources + `Log Analytics Contributor` or `Monitoring Reader`.

---

### 🌐 Azure – Storage & CDN

#### Blob Storage
2. **How do you restrict Blob Storage from public access?**
   - Disable public access at account level.
   - Configure Shared Access Signatures (SAS) or use Private Endpoints.

3. **How do you optimize Blob storage costs?**
   - Use Access Tiers: Hot, Cool, and Archive.

4. **What is a Shared Access Signature (SAS)?**
   - Token that grants limited access to storage resources without exposing keys.

5. **How do you troubleshoot app access issues to Blob Storage?**
   - Verify RBAC roles, SAS permissions, and firewall settings.

6. **How do you serve a static website with HTTPS using Azure Blob Storage?**
   - Enable static website hosting and front it with Azure CDN and HTTPS.

---

### 🔐 Azure – Security & Compliance

#### IAM & Secrets
1. **How do you securely store secrets in Azure?**
   - Use Azure Key Vault.

2. **How do you enforce MFA in Azure?**
   - Configure Conditional Access policies in Azure AD.

3. **What is an Azure Policy?**
   - Governance feature to enforce compliance with rules and effects.

#### Network Security
4. **What are key network security options in Azure?**
   - NSGs, Azure Firewall, Private Link, Application Gateway WAF, Azure DDoS Protection.

5. **How do you prevent Function App from accessing the internet?**
   - Use VNet Integration and disable public access.

#### Monitoring & Cost
6. **Your Azure bill spikes — how do you troubleshoot?**
   - Use Cost Analysis and Azure Advisor.
   - Set budgets and cost alerts.

---

### 📡 Azure – Connectivity & Access
1. **What is a Private Endpoint?**
   - Private IP access to PaaS services, avoiding public exposure.

---

### 📊 Azure – Monitoring & Cost Optimization
1. **How do you monitor Azure VM or Function performance?**
   - Use Azure Monitor, Application Insights, Log Analytics.

2. **What are Log Analytics workspaces?**
   - Centralized logging repositories.

3. **How do you optimize idle resources?**
   - Schedule shutdown for dev VMs.
   - Use Reserved Instances or Spot VMs.

---

### 🌀 Azure – Deployment & Automation
1. **How do you implement blue-green deployment in Azure?**
   - Use Deployment Slots in App Service or Traffic Routing in Azure Front Door.

2. **How do you define infrastructure as code in Azure?**
   - Use ARM templates or Bicep. Alternatively, use Terraform.

3. **What is Azure Event Grid?**
   - Event routing service for reactive automation.

4. **What is Azure Durable Functions?**
   - Stateful workflows in serverless architecture.

5. **What is a Function App extension?**
   - Extension bundles include bindings and triggers for runtime.

---