# Kubernetes Interview Questions & Answers

---

## ☸️ Pod & Container Troubleshooting

1. **A pod is stuck in CrashLoopBackOff — how do you troubleshoot it?**  
   Check pod logs (`kubectl logs`), describe pod events (`kubectl describe pod`), inspect readiness and liveness probes, and verify resource limits.

2. **A pod is stuck in Pending due to PVC issues — how do you troubleshoot?**  
   Check PersistentVolumeClaim status, verify StorageClass availability, and check for sufficient resources and binding issues.

3. **A pod cannot connect to a service in the same namespace — what could be wrong?**  
   Check service selectors, pod labels, NetworkPolicies, and DNS resolution inside the pod.

4. **A deployment is not scaling up as expected — what do you check?**  
   Inspect HPA (Horizontal Pod Autoscaler) config, cluster resource availability, deployment events, and pod quotas.

5. **A pod has high memory usage and gets OOMKilled — how do you handle this?**  
   Increase memory limits, optimize application memory usage, or add swap or node resources.

6. **How do you investigate a failed container in a terminated pod?**  
   Use `kubectl logs --previous`, check pod events and describe output.

7. **A node is NotReady — how do you investigate and fix it?**  
   Check node status (`kubectl get nodes`), node logs, kubelet health, and network connectivity.

---

## 📦 Stateful Workloads & Storage

8. **What is a StatefulSet and what happens to the PVC when it goes down?**  
   StatefulSet manages stateful apps with stable identities. PVCs remain bound and data persists even if pods are terminated.

9. **How do you migrate data from one PVC to another with minimal downtime?**  
   Use `kubectl cp` or run a migration job, then switch PVCs in the StatefulSet spec.

10. **How do you resize a Persistent Volume in Kubernetes?**  
    Use PVC resize (supported in many StorageClasses), edit PVC with new size, and ensure volume expansion is enabled.

---

## 🔄 Deployments & Rollouts

11. **You deployed a new version and traffic dropped — how do you roll back safely?**  
    Use `kubectl rollout undo deployment/<name>`.

12. **How do you upgrade a Kubernetes cluster with zero downtime?**  
    Upgrade control plane first, then nodes one-by-one; use draining and cordoning.

13. **How do you perform zero-downtime rolling deployments?**  
    Use `kubectl rollout` with proper readiness probes and update strategy.

14. **What is the difference between blue-green and canary deployments, and how do you implement them?**  
    Blue-green: deploy new version alongside old, then switch traffic.  
    Canary: gradually shift traffic to new version. Use Ingress controllers or service mesh.

15. **How do you set up canary deployments using ingress controllers?**  
    Configure weighted routing rules based on host/path and percentages.

---

## 🔐 Security & Secrets

16. **How do you handle certificate expiration?**  
    Monitor expiry, automate renewal (e.g., cert-manager), and reload pods.

17. **How do you rotate service account tokens and secrets securely?**  
    Use Kubernetes secrets, enable automatic token rotation or recreate secrets.

18. **What’s your approach to managing secrets securely?**  
    Use Kubernetes Secrets with encryption at rest, RBAC, or external vaults.

19. **How do you implement Pod Security Standards?**  
    Apply PodSecurity admission controller enforcing policies (restricted, baseline, privileged).

20. **What kind of encryption is used for storing secrets?**  
    Secrets are base64 encoded by default; enable EncryptionConfiguration for AES-CBC or KMS.

21. **How do you manage RBAC for developers who only need log access?**  
    Create Role with read access to pods and logs, bind it via RoleBinding.

22. **How do you ensure pod-to-pod communication is restricted?**  
    Use NetworkPolicies to restrict traffic by labels and namespaces.

---

## 🌐 Networking & DNS

23. **How does DNS resolution work inside Kubernetes?**  
    CoreDNS runs in cluster; pods resolve service names via cluster DNS.

24. **You can’t access a service using its ClusterIP — what are possible reasons?**  
    Service not created properly, endpoints missing, NetworkPolicies blocking traffic, or DNS issues.

25. **What tools do you use to debug network traffic?**  
    `kubectl exec` with tools like `curl`, `ping`, `tcpdump`, and `istioctl` for service mesh.

26. **How do you implement NetworkPolicies?**  
    Define ingress and egress rules to control pod communication.

---

## 🌍 Ingress & Traffic Management

27. **What steps do you take to debug ingress routing issues?**  
    Check ingress resource and controller logs, verify DNS and service backend.

28. **What is an Ingress Controller and how is it different from a Kubernetes Ingress?**  
    Ingress Controller is the implementation that processes Ingress resource rules and routes traffic.

29. **How do you configure TLS in an Ingress Controller?**  
    Use TLS secret references in ingress spec.

30. **How do you route traffic based on host and path using Ingress?**  
    Define rules in ingress spec with host and path match.

31. **How do you secure Ingress traffic with authentication?**  
    Use annotations for basic auth or integrate external auth providers.

32. **How do you troubleshoot Ingress 404 errors?**  
    Check ingress rules, service endpoints, and controller logs.

33. **What are the rate-limiting and request-size limits features in Ingress controllers?**  
    Use annotations to set limits on requests per second and max request size.

---

## 📈 Observability & Monitoring

34. **How do you monitor node and pod health?**  
    Use Prometheus with kube-state-metrics, node-exporter, and alertmanager.

35. **How do you alert on pod failures or restarts?**  
    Set alert rules in Prometheus or external tools.

36. **How do you log and trace traffic across microservices?**  
    Use EFK stack (Elasticsearch, Fluentd, Kibana), Jaeger or Zipkin for tracing.

---

## 👥 Multi-Tenancy & Resource Management

37. **How do you drain a node safely in a production cluster?**  
    Use `kubectl drain` to evict pods safely before maintenance.

38. **How do you isolate workloads from different teams or environments?**  
    Use namespaces and RBAC policies.

39. **How do you manage resource quotas and limit ranges?**  
    Apply ResourceQuota and LimitRange objects per namespace.

40. **How do you prevent one team from over-consuming cluster resources?**  
    Enforce quotas and monitor usage.

---

## ⚙️ Cluster Components & Operations

41. **What do you do when etcd usage grows uncontrollably?**  
    Compact and defragment etcd regularly.

42. **What is the workflow when you run kubectl?**  
    Client reads kubeconfig, authenticates, sends request to API server, which handles request.

---

## 🧩 Extensibility & Custom Resources

43. **What are the uses of CRDs?**  
    Extend Kubernetes API with custom resource types.

---

## 🔍 Security Scanning & CI/CD

44. **How do you scan container images for vulnerabilities in your pipeline?**  
    Use tools like Trivy, Clair, or Aqua integrated into CI pipelines.
