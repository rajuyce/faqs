# ⚙️ Istio Interview Questions & Answers

1. **How do you enable mTLS between services in Istio?**
- Enable PeerAuthentication with STRICT mode and DestinationRule with ISTIO_MUTUAL TLS.

2. **How do you manage canary deployments using Istio virtual services?**
- Use VirtualService for weighted traffic split between versions for gradual rollout.

3. **How do you debug issues with Envoy sidecars not starting?**
- Check pod logs/events, verify sidecar injection, service account permissions, and control plane health.

4. **How do you control traffic shifting using DestinationRules and VirtualServices?** 
- DestinationRules define subsets; VirtualServices route traffic between them using weights.

5. **What’s the difference between ingress gateway and Kubernetes ingress?** 
- Kubernetes Ingress is basic HTTP routing; Istio Ingress Gateway offers advanced proxy features integrated with Istio.

6. **How do you enable observability using Istio’s telemetry features?**
- Use Envoy proxies for metrics/logs/traces, integrate Prometheus, Grafana, Jaeger.

7. **How do you secure external traffic with Istio ingress gateway?**
- Configure Gateway with TLS, use AuthenticationPolicy/RequestAuthentication, and AuthorizationPolicy.

8. **How do you troubleshoot slow response times in an Istio-managed service?**
- Check Envoy logs, analyze tracing, monitor resources, verify mTLS/network overhead, review routing settings.
