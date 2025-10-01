# Network Troubleshooting

> some common commands for troubleshooting

```bash

kubectl get pods -o wide # shows Pod Ip addresses
kubectl get pods --show-labels  # shows Pod labels

kubectl get svc # shows services
kubectl describe svc ... # Shows Service successful connections to Pods
# Important - check selector and label are matching

kubectl get netpol -A # shows if anyt Network Policies are operational

kubectl describe ingress ... # shows ingress configuration
# if ingress shows no endpoints, ensure the Service it uses does show them
```