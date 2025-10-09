# Nginx Demo Deployment

Production-ready Kubernetes deployment demonstrating best practices.

## Features

- **High Availability**: 3 replicas with anti-affinity
- **Rolling Updates**: Zero-downtime deployments
- **Health Checks**: Liveness and readiness probes
- **Resource Management**: CPU and memory limits set
- **Service Discovery**: ClusterIP service for internal access

## Deployment
```bash
# Deploy to Kubernetes cluster
kubectl apply -f deployment.yaml

# Verify deployment
kubectl get deployments nginx-demo
kubectl get pods -l app=nginx-demo
kubectl get service nginx-demo

# Test the service
kubectl port-forward service/nginx-demo 8080:80
# Open browser: http://localhost:8080