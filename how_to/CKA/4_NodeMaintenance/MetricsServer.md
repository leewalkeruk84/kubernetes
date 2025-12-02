# Metrics Server

## Overview
Metrics Server is a cluster-wide aggregator of resource usage data (CPU and memory) from nodes and pods. It enables Kubernetes features like the Horizontal Pod Autoscaler (HPA) and the `kubectl top` commands.

Official References:
- Metrics Server GitHub (Full README, Installation, Troubleshooting): https://github.com/kubernetes-sigs/metrics-server
- Kubernetes HPA Walkthrough (links to Metrics Server): https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/

Exam Tip:
- Metrics Server is required for autoscaling.
- The Kubernetes documentation for autoscaling walk through links to the GitHub page.
  - https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/
  - look for 'To learn how to deploy the Metrics Server, see the metrics-server documentation.'
- On the GitHub README, scroll down to the Installation section for the `kubectl apply` command.

---

## Installation and Verification
```bash
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

# Verify installation
```bash
kubectl -n kube-system get pods
```
# If pods are not starting properly, check logs:
```bash
kubectl -n kube-system logs metrics-server-<pod-name>

# Example log output:
# I1103 13:09:10.037410       1 server.go:192] "Failed probe" probe="metric-storage-ready" err="no metrics to serve"
# E1103 13:09:15.326467       1 scraper.go:149] "Failed to scrape node" err="Get \"https://192.168.49.2:10250/metrics/resource\": tls: failed to verify certificate: x509: cannot validate certificate for 192.168.49.2 because it doesn't contain any IP SANs" node="minikube"
```

## Fixing TLS Issues (Minikube or Local Clusters)
```bash
kubectl -n kube-system edit deployments.apps metrics-server

# In the args section, add --kubelet-insecure-tls
# Original:
#   - --cert-dir=/tmp
#   - --secure-port=10250
#   - --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
#   - --kubelet-use-node-status-port
#   - --metric-resolution=15s
#
# Updated:
#   - --cert-dir=/tmp
#   - --secure-port=10250
#   - --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
#   - --kubelet-use-node-status-port
#   - --metric-resolution=15s
#   - --kubelet-insecure-tls
```

# After saving, watch for the new pod to start
```bash
kubectl get pods -n kube-system -w
# Wait until the Metrics Server pod shows Running 1/1
```

## Usage
```bash
kubectl top nodes
kubectl top pods
```
# Note: These commands will not work without Metrics Server running properly.
