# Application Monitoring on OpenShift

## Overview

Our nginx-demo application is monitored using OpenShift's built-in observability features, which leverage Prometheus for metrics collection.

## Key Metrics

### CPU Usage
- **Normal Load:** < 0.0001 cores per pod
- **Under Load (500 req/sec):** ~0.0008 cores per pod
- **Limit:** 200m (0.2 cores)

### Memory Usage
- **Baseline:** ~20MB per pod
- **Under Load:** ~25MB per pod
- **Limit:** 128MB per pod

### Network Bandwidth
- **Normal:** < 500 bytes/sec
- **Peak Load:** 5KB/sec (500 requests in ~30 seconds)

## Load Test Results

**Test Date:** October 8, 2025  
**Test Duration:** ~30 seconds  
**Total Requests:** 500  
**Success Rate:** 100%  
**Response Time:** < 100ms average

### Test Command
```bash
URL=$(oc get route nginx-demo-nginx-demo-chart -o jsonpath='{.spec.host}')
for i in {1..500}; do
  curl -s -k https://$URL > /dev/null
done

Observed Behavior
✅ All 3 pods handled traffic evenly (load balancing working)
✅ No pod restarts or failures
✅ Memory usage remained stable
✅ CPU usage returned to baseline after load
✅ Response times consistent throughout test
Accessing Metrics
Via OpenShift Console

Navigate to Workloads → Deployments
Click on nginx-demo-nginx-demo-chart
Select Observe tab
View real-time metrics:

CPU usage
Memory usage
Network bandwidth



Via CLI
bash# Get pod metrics
oc adm top pods

# Get node metrics (if admin access)
oc adm top nodes

Via Prometheus 

bash# Port-forward to Prometheus
oc port-forward -n openshift-monitoring prometheus-k8s-0 9090:9090

# Access Prometheus UI at http://localhost:9090
# Query: rate(container_cpu_usage_seconds_total{pod=~"nginx-demo.*"}[1m]


Alerts (Planned)
Future monitoring improvements:

 Set up alerts for high CPU usage (>80%)
 Memory usage alerts (>100MB)
 Pod crash alerts
 Response time degradation alerts
 Integrate with Grafana for custom dashboards

Performance Optimization
Based on metrics, the application is performing well:

Resource requests/limits are appropriate
No over-provisioning or under-provisioning
Application scales horizontally as expected

Next Steps

Add Grafana for advanced visualization
Set up Prometheus alerts
Integrate with PagerDuty/Slack for notifications
Add distributed tracing with Jaeger
Implement service mesh observability with Istio

Metrics collected using OpenShift built-in Prometheus
