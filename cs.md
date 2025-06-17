### 1. Operating system, basic networking and scripting
#### a) While managing Linux-based servers, you come across an 8-core box taking very long to establish a connection (via secure shell). Once in, you notice the following:
```
load average: 99.53, 97.19, 91.12
```
- Explain what the above means
- Speculate what could be causing it (give examples)
- Theorise how you would solve one of the cases you’ve just described, hence returning the box to normal operation

**Load average: 99.53, 97.19, 91.12** means:
- These are the 1, 5, and 15-minute averages of the number of processes waiting to run.
- On an **8-core server**, anything above 8 is "overloaded".
- Here, ~100 means **massive CPU contention**.
**Possible Causes:**
- **CPU-bound processes**: e.g., infinite loops in code or high-computation tasks.
- **Process fork bomb**: A script or bug spawns too many processes.
- **Stuck kernel threads or I/O wait**: Blocking resources even if CPU looks busy.
- **Misbehaving applications**: Logging too much, spinning on errors, etc.
**Solution (example: CPU hogging process):**
1. `top` or `htop` to identify high CPU processes.
2. Use `ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%cpu | head` to see CPU consumers.
3. Investigate the culprit process.
4. If malicious/stuck: `kill -9 <pid>`.
5. For permanent fix:
 - Patch the app
 - Add systemd limits (`CPUQuota`, `MemoryLimit`)
 - Use `nice`/`cgroups` to restrict bad actors
---
#### b) You have created a lightweight script for pushing metrics into a centralised time- series database. Testing it locally on your laptop, everything works fine. Running it on a server, however, throws back the following error message:
```
getaddrinfo for my-cool-metrics.server.internal failed: Name or service not known
```
- Explain what the error message is telling you
- The endpoint is correct, propose a solution to making your script work
**Explanation:**
- The script is trying to resolve a DNS name.
- It can't find **`my-cool-metrics.server.internal`** in DNS or `/etc/hosts`.
**Fixes:**
1. Check if `/etc/resolv.conf` has working DNS servers.
2. Test DNS manually: `nslookup my-cool-metrics.server.internal`
3. Add the hostname to `/etc/hosts` (if static):
 `10.1.2.3 my-cool-metrics.server.internal`
4. If its internal to a cloud (e.g., AWS), ensure:
 - Instance is in correct VPC
 - Has access to the correct resolver
5. Ensure no proxy/DNS split issues.
---
#### c) A web server running a standard reverse-proxy setup (nginx + backend running on the same box, communicating over TCP) sporadically returns 502 response codes. CPU load, load average, memory utilisation, disk space and I/O metrics are within standard operational ranges. The multi-threaded, back-end application, running on the server, is self-sufficient (does not require external services to operate) and is capable of reliably handling up to 1000 qps. Assuming the server is on the open internet and is a known authority for randomly providing baby names, speculate why nginx could be returning 502 responses.

**Scenario:**
- Backend app & NGINX on same box.
- Backend can handle 1000 qps.
- Sporadic `502 Bad Gateway`.
**502 means:** NGINX tried to talk to backend but failed.
**Possible Causes:**
1. **App takes too long to respond sometimes** NGINX timeout
2. **Backend port not listening or crashes intermittently**
3. **Nginx config issues**:
 - `proxy_pass` points to wrong port
 - `keepalive` settings too low
4. **Resource limits** (ulimit, systemd `LimitNOFILE`)
5. **DNS resolution lag for backend name**
6. **Socket backlog** overflow during traffic bursts
**Recommendation:**
- Check `nginx/error.log`
- Tune timeouts:
 ```
 proxy_connect_timeout 5s;
 proxy_read_timeout 10s;
 ```
- Use `netstat -plant | grep :<port>` or `ss -ltnp`
- Increase `worker_connections`, `ulimit -n`
---
#### d) You need to create an automated approach to making centrally-managed (remote service) environment variables available to a group of Linux-based servers (at the shell level). The allowed synchronisation drift is 10 seconds. Propose a solution using BASH, Python or any scripting language of your choice. Assuming the centralised storage offers a simple HTTP endpoint (basic authentication, takes only variable names as parameters - comma-separated string, takes 2 seconds to respond), provide a simple script for retrieving the values and pulling them to the servers. Describe your solution’s workflow.
**Goal:** Sync env vars from a remote HTTP endpoint to Linux servers
**Approach:** Use Python + systemd timer
**Python Script (`env_sync.py`)**
```python
#!/usr/bin/env python3
import requests
from base64 import b64encode
USERNAME = "myuser"
PASSWORD = "mypassword"
ENV_VARS = ["API_KEY", "LOG_LEVEL"]
REMOTE_URL = "https://env-sync.example.com/vars"
LOCAL_ENV_FILE = "/etc/profile.d/remote_env.sh"
auth_header = b64encode(f"{USERNAME}:{PASSWORD}".encode()).decode()
headers = {"Authorization": f"Basic {auth_header}"}
params = {"vars": ",".join(ENV_VARS)}
Linux Infrastructure & Troubleshooting Guide
try:
 response = requests.get(REMOTE_URL, headers=headers, params=params, timeout=5)
 response.raise_for_status()
 with open(LOCAL_ENV_FILE, "w") as f:
 for line in response.text.strip().splitlines():
 if "=" in line:
 key, val = line.split("=", 1)
 f.write(f'export {key}="{val}"\n')
except Exception as e:
 print(f"Sync failed: {e}")
```
**Automation:**
- Use `systemd` timer every 10s or cron with `sleep` intervals.
- Add to `/etc/profile.d/` to make available to all shells.
---
#### e) You’re setting up a small cluster of servers (3-tier deployment, 4 servers - 1:2:1) and you need to decide how you would configure their networking. Provide a simple diagram to illustrate how you would achieve this (assume you’re not working with physical servers), indicating connectivity links and assigned IP addresses.
**Architecture:**
- Frontend: `10.0.0.10`
- App Servers: `10.0.0.21`, `10.0.0.22`
- DB: `10.0.0.30`
**Connectivity:**
Frontend App Servers DB
**Firewall rules:** allow specific ports only for communication between tiers.
---


### 2. Amazon Web Services

#### You’re running a multi-account setup (star topology). Each account serves a different area of the Software Development Lifecycle (development, testing, staging, production, monitoring, etc.). Only one of them takes any inbound traffic from the open internet.
#### a) Assuming you’ve got the task of organising the AWS accounts from scratch, what kind of networking topology would you use - as suggested above or different? Explain how you would solve the cross-account connectivity challenge and why (give examples to support your arguments). Provide details on the individual account setup (hint: go with one VPC per account), including networking configuration, indication of which account can “speak” to which of the rest and how you would control the traffic flow (hint: don’t go deeper than VPC and its directly related components, e.g. subnets, routing tables, security groups, etc.)

#### Proposed Networking Topology: Star Topology (Hub-and-Spoke Model)

**Why Star Topology?**
- Centralizes shared services (e.g., NAT, DNS, logging).
- Enforces environment separation (dev, test, prod, etc.).
- Enhances security and compliance.
- Simplifies cross-account connectivity and network control.

#### 🏗️ Individual Account and VPC Setup

| Account         | VPC CIDR      | Role                       | Internet Access | Connects To         |
|----------------|---------------|----------------------------|------------------|---------------------|
| Network (Hub)  | 10.0.0.0/16   | Central TGW, NAT, DNS, logs| ✅               | All                 |
| Development    | 10.1.0.0/16   | Dev workloads              | ❌               | Staging, Monitoring |
| Testing        | 10.2.0.0/16   | QA/Testing                 | ❌               | Staging, Monitoring |
| Staging        | 10.3.0.0/16   | Pre-prod                   | ❌               | Production          |
| Production     | 10.4.0.0/16   | Live workloads             | ✅ (ingress only)| Staging, Monitoring |
| Monitoring     | 10.5.0.0/16   | Monitoring & logging       | ❌               | All                 |

#### Cross-Account Connectivity: AWS Transit Gateway (Preferred)

- TGW hosted in Network Account.
- VPCs attach to TGW.
- TGW route tables define who can talk to whom.
- Example: Dev → Monitoring allowed; Dev → Prod blocked.

####  Network Configuration
- **Subnets**: Public for ingress (only in Prod), private for app workloads.
- **Routing Tables**: Private subnets route to TGW or NAT in Network VPC.
- **Security Groups**: Use cross-account SG referencing for precise control.
- **NACLs**: Optional, use only if strict control needed.

#### Internet Exposure
- Only Production has ALB/NLB in a public subnet.
- Others use centralized NAT via TGW.

---

#### b) There are multiple groups of employees, requiring different levels of access permissions to each of the accounts. How would you architect an IAM-based solution to grant and manage these? Explain the benefits of the suggested solution and list any potential drawbacks or considerations.

#### Centralized Access via AWS IAM Identity Center (AWS SSO)

**Key Components:**
- AWS Organizations: Centralized account and OU management.
- IAM Identity Center: Central access control and user assignment.
- Identity Source: AWS directory or external (e.g., Azure AD).
- Permission Sets: Predefined roles reused across accounts.

####  Example Access Matrix

| Group           | Accounts               | Access Type         |
|-----------------|------------------------|---------------------|
| Dev Team        | Dev, Test, Staging     | Admin               |
| QA Team         | Test, Staging          | Read + Execute      |
| Ops Team        | All                    | Least Privilege Ops |
| Monitoring Team | All                    | ReadOnly            |
| SecOps          | All                    | SecurityAudit       |

#### Benefits

- Centralized control
- Scalable and auditable
- Least privilege enforcement
- Temporary, session-based access
- Compatible with SAML/SSO

####  Drawbacks

- Regional IAM Identity Center
- CLI requires `aws configure sso`
- Initial setup complexity
- Needs IdP integration skills

---

#### c) All in-house-built services deployed to the environments need to use local (to their host AWS account) AWS resources, while some of them also need to be able to access AWS resources from other AWS accounts. Explain how you would manage the permissions, available to each of the services (regardless of the platform they’re running on - could be EC2, ECS, EKS, Fargate, etc.) and how you would authorise access to the in-house-built services/applications.

#### Strategy: IAM Role-Based with Cross-Account AssumeRole

- Services use **IAM roles** (EC2 instance profile, EKS IRSA, Fargate task roles).
- For cross-account, assume IAM roles in target accounts.
- Use **STS AssumeRole**, trust policies, and resource policies.

#### Flow Example

```
Dev Account (1111)
├── service-a-role
└── Assumes → cross-reader-role (in Monitoring)

Monitoring Account (2222)
└── cross-reader-role trusts service-a-role@1111
```

#### IAM Policies

- Source role allows `sts:AssumeRole`
- Target role trusts source role
- Temporary credentials used

#### Platform-Specific

| Platform | Role Attachment Method          |
|----------|---------------------------------|
| EC2      | Instance Profile                |
| ECS      | Task Role                       |
| Fargate  | Task Execution Role             |
| EKS      | IRSA (IAM Role for Service Acct)|
| Lambda   | Execution Role                  |

#### Benefits

- Secure, temporary credentials
- Least privilege by design
- Full audit via CloudTrail
- Platform-agnostic

#### Considerations

- STS latency
- Role sprawl
- Complexity with many accounts
- Regular auditing needed


## 3. Containerisation

You’ve got the task of putting together a Docker-based container image, which would be used as the foundation of a broad variety of microservices. It has to be streamlined, reliable and multi-architecture compatible (for the sake of simplicity, needs to run on x86_64 and ARM64).

---

### a) Propose a plan how you would approach the task, gather initial requirements and nominate stakeholders

####  Understand the Objective
- Goal: Create a general-purpose, production-grade Docker base image that:
  - Is minimal and secure
  - Supports x86_64 and ARM64 architectures
  - Can be reused as the foundation for various internal microservices
  - Supports common language runtimes or utilities as needed (Node.js, Python, Java, Go, etc.)
  - Ensures fast startup times, low image size, and long-term maintainability

####  Identify and Engage Stakeholders

| Stakeholder             | Role / Input Required |
|-------------------------|------------------------|
| Platform/DevOps Team    | Define CI/CD pipeline integration, base OS/image standard, security scanners |
| Security Team           | Validate security posture, CVE scanning tools, hardening practices |
| Developers              | Provide runtime/language/library requirements for various microservices |
| QA/Performance Team     | Ensure image meets functional/performance benchmarks across platforms |
| Architects              | Align with the architectural vision (e.g., OCI compliance, layering strategy) |

####  Gather Initial Requirements

**Functional Requirements:**
- Supported OS base (e.g., Alpine, Debian Slim, or Distroless)
- Runtime support: PHP, Python, Node, Java, Go
- Common tooling: curl, bash, git, etc.
- Health check capability
- Logging/output format support

**Security Requirements:**
- Non-root user
- Regular vulnerability scanning (e.g., Trivy, Grype)
- Secure default file permissions
- Signature and provenance support (e.g., Sigstore, Cosign)

**Build Requirements:**
- Multi-architecture builds: linux/amd64, linux/arm64
- Reproducible builds (no dynamic timestamps, use of build args)
- Automated build pipeline

**Image Delivery:**
- Hosted in Docker registry (e.g., AWS ECR, GitHub Container Registry)
- Tagged by version and architecture
- CI integration with semantic versioning

####  High-Level Steps in the Plan

1. Baseline Research: Evaluate minimal base images
2. PoC Image Build: Build a multi-arch Dockerfile using `docker buildx`
3. Image Hardening: Add non-root user, patch CVEs, scan with tools like Trivy
4. Add Utility Layers: Install common dependencies
5. Automation: Set up GitOps pipeline with linting, security scan, test, and publish
6. Multi-Arch Build Test
7. Documentation
8. Stakeholder Review
9. Rollout Strategy

---

### b) Provide an example build file (Dockerfile), which would create a container image for a standard PHP8-based application, which would in turn be communicating with a PostgreSQL relational database and a Redis key-value store.

```Dockerfile
FROM --platform=$BUILDPLATFORM php:8.2-fpm-slim AS base

LABEL maintainer="you@example.com"

ENV TZ=UTC     DEBIAN_FRONTEND=noninteractive     PHP_OPCACHE_VALIDATE_TIMESTAMPS=0

RUN apt-get update && apt-get install -y --no-install-recommends     git curl unzip libpq-dev libzip-dev libonig-dev libxml2-dev pkg-config libssl-dev     && docker-php-ext-install -j$(nproc) pdo pdo_pgsql pgsql zip opcache sockets     && pecl install redis && docker-php-ext-enable redis     && apt-get clean && rm -rf /var/lib/apt/lists/*

WORKDIR /var/www/html
COPY . .

RUN groupadd -g 1000 appuser &&     useradd -u 1000 -g appuser -s /bin/bash -m appuser

RUN chown -R appuser:appuser /var/www/html

USER appuser

EXPOSE 9000

CMD ["php-fpm"]
```

**Build for multi-arch using:**
```bash
docker buildx build --platform linux/amd64,linux/arm64 -t your-registry/php8-app:latest --push .
```

---

### c) Given the container image would be used in a variety of environments (locally by developers, for automated testing, beta deployments, etc.), explain how you would build, store, manage and maintain it (hint: remember it would be pulled from different locations). Provide a diagram to visualise the proposed workflow.

## Container Image Lifecycle Management Strategy

To ensure consistent, secure, and efficient usage across local development, CI/CD testing, and production deployments, here’s a streamlined workflow with a visual diagram.

 #### 1. Workflow Overview
Key Stages
##### Build: Multi-arch image with versioned tags.

##### Store: Central registry with access controls.

##### Deploy: Environment-specific pulls (dev/test/prod).

##### Maintain: Automated updates and vulnerability scans.


Diagram















#### 2. Implementation Details
##### A. Build Phase
Tools:

docker buildx: Multi-architecture builds (x86_64 + ARM64).

GitHub Actions/GitLab CI: Automated pipelines.

Process:

Versioning: Tag images with:

:latest (for dev/testing).

:v1.2.3 (semantic versioning for production).

Git SHA (:commit-abc123) for traceability.

Multi-Arch Build Command:

```
docker buildx build --platform linux/amd64,linux/arm64 \
  -t my-registry/php-app:latest \
  -t my-registry/php-app:v1.0.0 \
  --push .
```
##### B. Storage & Registry
Central Registry Options:

Registry	Use Case
AWS ECR	Production workloads in AWS.
GitHub (GHCR)	Open-source or GitHub-native teams.
Harbor	Self-hosted with RBAC and scanning.
Best Practices:

Access Control:

Developers: Read-only for :latest.

CI/CD: Read/write for tagged releases.

Storage Retention:

Keep :latest and last 5 versions.

Archive older images to cold storage (e.g., S3).

##### C. Environment-Specific Pulls
Environment	Image Tag	Pull Policy
Local Dev	:latest	Always (fresh builds).
Automated Tests	:commit-abc123	IfNotPresent.
Production	:v1.2.3	Never (immutable).
Example Kubernetes Deployment:

yaml
```
containers:
- name: php-app
  image: my-registry/php-app:v1.0.0
  imagePullPolicy: IfNotPresent
```
##### D. Maintenance & Security
Automated Tools:

Trivy/Clair: Scan images for CVEs in CI/CD.

RenovateBot: Update base images (e.g., php:8.2-alpine).

Weekly Rebuilds: Refresh :latest to patch CVEs.

Rollback Plan:

Pin production to immutable tags (v1.2.3).

Rapid revert via Kubernetes rollback:

```
kubectl rollout undo deployment/php-app
```
#### 3. Key Benefits
- Consistency: Same image runs everywhere.
- Security: Scanned, non-root, minimal OS.
- Scalability: Multi-arch + registry caching.
- Traceability: Git SHA and semantic versioning.

 #### 4. Potential Challenges & Mitigations
Challenge	Solution
Registry downtime.	Cache images in local artifact storage.
Slow ARM64 builds.	Use Graviton runners in CI.
CVE backlogs.	Enforce weekly rebuilds.
Final Workflow Summary
Developers pull :latest from a local cache (e.g., docker pull localhost:5000/php-app:latest).

CI/CD builds, scans, and pushes to a central registry.

Environments pull versioned images with strict policies.

Security tools enforce compliance and updates.

This ensures a reliable, secure, and scalable container lifecycle.

