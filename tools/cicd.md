## CI/CD Interview Scenarios – Detailed Answers

### Scenario 1
**Issue:** CI pipeline fails after a merge; error log shows failed unit test but it passes locally.  
**Solution:**
- Check for environment differences (e.g., Python/Node versions, dependencies).
- Review `.gitlab-ci.yml` or workflow definition for test runner settings.
- Run tests inside a container or CI-like environment locally using Docker.
- Check if the test is non-deterministic or relies on time/external services.
- Add verbose logs or debug the test failure in CI logs.

---

### Scenario 2
**Issue:** QA deployment works but production fails due to infra differences.  
**Solution:**
- Use Infrastructure as Code (Terraform/CloudFormation) to standardize environments.
- Compare environment variables, secrets, and configs.
- Introduce automated tests post-deploy to validate consistency.
- Run smoke tests in a staging environment identical to prod.

---

### Scenario 3
**Issue:** Bug in deployment, need rollback.  
**Solution:**
- Use versioned deployments via Helm, ArgoCD, or GitOps.
- Rollback using Helm: `helm rollback <release> <revision>`.
- Maintain container image tags (e.g., `v1.2.0`) for traceability.
- If DB changes were involved, ensure reversible migrations.

---

### Scenario 4
**Issue:** CI pipeline takes 40+ mins.  
**Solution:**
- Profile pipeline stages to identify bottlenecks.
- Use caching for dependencies and Docker layers.
- Parallelize tests and builds.
- Use conditional jobs or matrix strategy.
- Run only impacted jobs using path filters or changed files.

---

### Scenario 5
**Issue:** API key accidentally committed.  
**Solution:**
- Immediately rotate the API key.
- Remove key from Git history (e.g., `git filter-branch`, `BFG Repo-Cleaner`).
- Add secrets scanning tools (e.g., GitLeaks, TruffleHog) in CI.
- Use environment secrets management (e.g., HashiCorp Vault, AWS Secrets Manager).

---

### Scenario 6
**Issue:** Add manual approval step.  
**Solution:**
- Add a manual `approval` stage in tools like GitHub Actions, GitLab CI, or ArgoCD.
- Use `environment: production` with `reviewers` in GitHub Actions.
- Document approval workflows and notify reviewers.

---

### Scenario 7
**Issue:** Zero downtime deployment.  
**Solution:**
- Use Blue-Green or Canary deployment.
- Kubernetes: use rolling updates, readiness probes, `maxSurge`, `maxUnavailable`.
- Load balancer should drain old pods only after new pods are healthy.

---

### Scenario 8
**Issue:** Deployment with DB schema changes.  
**Solution:**
- Use versioned migrations (Flyway, Liquibase).
- Apply backward-compatible changes first.
- Deploy app after migration.
- Roll forward if possible instead of rollback.

---

### Scenario 9
**Issue:** High reliability app: Blue-Green vs Canary?  
**Solution:**
- **Blue-Green**: Simple switch; useful for stateful apps, instant rollback.
- **Canary**: Gradual rollout to subset of users; better for minimizing risk.
- **Choose Canary** if fine-grained control, monitoring, and rollback are critical.

---

### Scenario 10
**Issue:** Migrate UI-based pipelines to code.  
**Solution:**
- Export existing configurations.
- Recreate pipeline using YAML (`.gitlab-ci.yml`, `.github/workflows/*.yml`).
- Store pipeline code in Git.
- Use CI linting and validation tools.

---

### Scenario 11
**Issue:** Bug appears only in production.  
**Solution:**
- Enable verbose logging in production.
- Use feature flags to isolate.
- Compare config/staging/infra between environments.
- Add production parity staging setup.

---

### Scenario 12
**Issue:** Frequent merge conflicts.  
**Solution:**
- Encourage short-lived branches.
- Use trunk-based development.
- Merge/rebase frequently.
- Add pre-merge validations and automatic rebase pipelines.

---

### Scenario 13
**Issue:** Create/destroy temp environments per PR.  
**Solution:**
- Use Infrastructure as Code (Terraform) with dynamic namespaces.
- Automate via GitHub Actions or GitLab pipelines on PR events.
- Tear down on PR close/merge.

---

### Scenario 14
**Issue:** Feature branches pass CI, main fails.  
**Solution:**
- Check if merges introduce conflicting logic.
- Use merge trains or preview branches.
- Ensure tests run on merged state.

---

### Scenario 15
**Issue:** Onboard 10+ teams to shared CI/CD.  
**Solution:**
- Create pipeline templates and reusable components.
- Define governance, shared standards.
- Use GitHub Actions Reusable Workflows or GitLab includes.
- Offer onboarding documentation and support.

---

### Scenario 16
**Issue:** Pipeline breaks due to dependencies.  
**Solution:**
- Pin versions of tools and dependencies.
- Use lock files and package registries.
- Add alerting for upstream changes.

---

### Scenario 17
**Issue:** Trace artifact to commit.  
**Solution:**
- Embed Git commit hash in artifact metadata (e.g., Docker label).
- Tag images with commit SHA.
- Use SBOMs (Software Bill of Materials).

---

### Scenario 18
**Issue:** Secure against supply chain attacks.  
**Solution:**
- Use SLSA or sigstore for build provenance.
- Sign artifacts.
- Use isolated runners.
- Use dependency scanning and SBOMs.

---

### Scenario 19
**Issue:** Track who/when/where for deployments.  
**Solution:**
- Use audit logging tools (e.g., ArgoCD audit logs, GitHub Actions logs).
- Include deployment info in commit messages or changelogs.
- Integrate with external logging systems (ELK, Datadog).

---

### Scenario 20
**Issue:** Monorepo runs all jobs for all changes.  
**Solution:**
- Use path filters to run relevant jobs (e.g., `if: changes.include('path/')`).
- Tools: `nx affected`, `bazel`, or GitHub Actions `paths`.
- Cache results to avoid repeated builds.

---

