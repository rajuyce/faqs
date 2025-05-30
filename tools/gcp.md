### ☁️ GCP – Compute & Networking

#### VM Instances & Networking
1. **Your GCP VM instance is unreachable — what do you check?**
   - Firewall rules, subnet routing, instance health, startup script errors.

2. **How do you design HA compute architecture in GCP?**
   - Use Managed Instance Groups across zones with Load Balancer.

3. **App not reachable despite running VM — what do you debug?**
   - Check firewall, instance group backend settings, app port, and load balancer config.

4. **How do you automate scaling in GCP?**
   - Use Managed Instance Groups with autoscaling based on metrics.

5. **What’s the best way to restore a deleted VM?**
   - Use custom images and snapshot backups.

6. **How do you automate patching?**
   - Use OS patch management and patch jobs via OS Config.

8. **What GCP services require VPC?**
   - Compute Engine, GKE, Cloud SQL (private IP), Cloud Functions (2nd Gen), etc.

9. **How do you connect multiple VPCs securely?**
   - VPC Peering, Shared VPC, or VPN/IPSec tunnels.

#### IAM & Service Accounts
10. **Can a VM use multiple service accounts?**
    - No, only one service account per VM, but IAM scopes can be extended.

11. **What permissions are needed for a Cloud Function to access GCS?**
    - Assign storage roles to the function’s service account.

---

### 🌐 GCP – Storage & CDN

#### GCS Buckets
2. **How do you ensure GCS is not publicly accessible?**
   - Disable Uniform Bucket-Level Access and review IAM roles.

3. **How do you optimize GCS cost?**
   - Use Nearline, Coldline, and Archive storage classes.

4. **What is Signed URL and Signed Policy?**
   - Temporary access tokens to GCS objects.

5. **How to debug an app’s access failure to GCS?**
   - Check service account permissions and bucket IAM.

6. **How do you serve a static website via HTTPS with GCS?**
   - Host in GCS, use Cloud CDN and Cloud Load Balancer with SSL.

---

### 🔐 GCP – Security & Compliance

#### Secrets & IAM
1. **How do you store secrets in GCP?**
   - Use Secret Manager.

2. **How do you enforce MFA in GCP?**
   - Use Context-Aware Access and Identity-Aware Proxy (IAP).

3. **What is an Org Policy in GCP?**
   - Constraints to enforce compliance across organization resources.

#### Network Security
4. **How do you secure GCP network access?**
   - Use firewall rules, VPC Service Controls, and Private Service Connect.

5. **How to restrict Cloud Function from internet access?**
   - Use VPC connector in private subnet with no internet gateway.

---

### 📡 GCP – Connectivity & Access
1. **What is a Private Service Connect?**
   - Secure private access to Google services or third-party services over internal IPs.

---

### 📊 GCP – Monitoring & Cost Optimization
1. **How do you monitor and alert on GCP workloads?**
   - Use Cloud Monitoring and Cloud Logging.

2. **How do you reduce cost of idle workloads?**
   - Use autoscaling, preemptible VMs, shutdown scripts.

---

### 🌀 GCP – Deployment & Automation
1. **How do you implement blue-green deployments in GCP?**
   - Use Traffic splitting in Cloud Run or canary rollout in GKE.

2. **How do you define infrastructure in GCP?**
   - Use Deployment Manager or Terraform.

3. **What is Eventarc?**
   - Event routing from Google Cloud services to Cloud Run, Workflows.

4. **What are Workflows in GCP?**
   - Serverless orchestration of services and APIs.

---