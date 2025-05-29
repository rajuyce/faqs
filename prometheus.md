# Prometheus / Monitoring Interview Questions & Answers

---

## 📈 Prometheus / Monitoring

1. How do you troubleshoot high latency in an application using Prometheus?  
- Check latency metrics, resource correlations, dependencies, and traces.

2. You receive an alert about high CPU usage but top shows it's normal. What do you do?  
- Verify metric source, check CPU throttling, confirm alert freshness.

3. Prometheus stops scraping a target — how do you investigate and fix it?  
- Check Prometheus Targets UI, network, and endpoint availability.

4. Metrics are missing from Prometheus — what could be the causes?  
- Misconfiguration, changed endpoints, network issues, metric name changes.

5. Grafana panels show “No Data” — how do you troubleshoot?  
- Check datasource connection, query, and scrape data availability.

6. How do you scale Prometheus for large clusters?  
- Federation, Thanos/Cortex, workload partitioning.

7. How do you handle a growing Prometheus PVC?  
- Retention policies, dynamic PVC resize, remote storage.

8. What is a safe way to reduce Prometheus PVC size?  
- Reduce retention, backup, use storage compaction.

9. How do you manage long-term storage for Prometheus?  
- Remote storage (Thanos, Cortex), object storage.

10. How do you handle high cardinality metrics?  
- Avoid high-cardinality labels, use aggregation.

11. How would you monitor a multi-region application using Prometheus?  
- Per-region Prometheus, federation, region labels.

12. Explain the push vs. pull model in Prometheus.  
- Pull: Prometheus scrapes targets. Push: Pushgateway for short-lived jobs.

13. How do you set up Prometheus service discovery in Kubernetes/EKS?  
- Use Kubernetes service discovery configs.

14. How do you create alerts in Prometheus?  
- Define alerting rules using PromQL.

15. How do you alert on memory usage exceeding 80% for 5 minutes?  
- Use `avg_over_time()` PromQL with threshold.

16. How do you avoid false positive alerts?  
- Proper thresholds, silences, tuning rules.

17. How do you route alerts to different teams using Alertmanager?  
- Use routing trees based on labels.

18. How do you suppress alerts during maintenance?  
- Use silences and inhibit_rules.

19. Describe a real incident where Prometheus alerting helped.  
- Early detection of memory leak prevented downtime.

20. How do you reduce alert noise?  
- Aggregation, grouping, tuning.

21. How do you create dashboards in Grafana?  
- Connect datasource, create panels with PromQL.

22. How do you monitor HTTP error rates in Grafana?  
- Query error codes, calculate error ratio.

23. How do you show metrics from multiple Prometheus sources in one Grafana dashboard?  
- Multiple datasources and mixed queries.

24. How do you securely share Grafana dashboards?  
- Snapshots, user permissions, organizations.

25. How do you monitor Java or Python apps with Prometheus?  
- Use language exporters and instrument code.

26. How do you expose custom metrics to Prometheus?  
- Instrument code, expose /metrics endpoint.

27. What exporters have you used?  
- node_exporter, blackbox_exporter, mysql_exporter, kube-state-metrics.

28. How do you secure exporters?  
- Network policies, auth proxies, TLS.

29. What metrics do you track for app availability and reliability?  
- Uptime, error rates, latency, restarts.

30. Prometheus is using too much memory — how do you optimize it?  
- Retention tuning, reduce cardinality, scrape interval adjustments.

31. How do you monitor and alert on pod restarts or OOM kills?  
- Use restart and termination metrics with alerts.

32. How do you monitor end-to-end latency from API Gateway to backend?  
- Instrument components and use tracing.

33. How do you differentiate infra vs. app issues using monitoring?  
- Separate infra and app metrics.

34. What are common Prometheus usage mistakes?  
- High cardinality, scraping mistakes, no remote storage, poor alert tuning.
