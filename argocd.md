# Argo CD Interview Questions & Answers

---

## 🚀 ArgoCD

1. **What are the different sync strategies in Argo CD?**  
   - Automatic sync: Automatically syncs on Git changes.  
   - Manual sync: Sync triggered manually.  
   - Hook-based sync: Supports pre-sync, sync, post-sync hooks for custom logic.  
   - Sync Waves: Control order of resource application.

2. **What is an ApplicationSet in Argo CD and when would you use it?**  
   ApplicationSet generates multiple Argo CD applications from templates and input parameters, useful for deploying across multiple clusters or environments dynamically.

3. **How do you manage multiple Kubernetes clusters with Argo CD?**  
   Register multiple clusters using `argocd cluster add`. Target clusters are specified in application manifests via `destination.server`.

4. **How does Argo CD handle deployments when using the latest tag in manifest files?**  
   It does not detect changes because manifests don’t change. Use image tag locking or tools like Argo CD Image Updater to update manifests in Git.

5. **How do you perform GitOps-based deployments using Argo CD?**  
   Store manifests/Helm charts in Git, create Argo CD Application pointing to repo, Argo CD monitors and syncs cluster state to Git.

6. **How do you inject secrets securely using Argo CD?**  
   Use Kubernetes Secrets, integrate with Vault/SOPS/Sealed Secrets, avoid raw secrets in Git, or use argocd-vault-plugin for dynamic secret injection.
