# Using Port Forwarding for Direct Application Access

`kubectl port-forward` can be used to connect to applications for anlayzing and troubleshooting

It forwards traffic coming in to a local port on the kubectl client machine to a port that is available in a Pod

Using port forwarding allows you to test application access without the need to configure Services and Ingress

Use `kubectl port-forward mypod 1234:80` to forward local port 1234 to Pod port 80

To run in the backgroud, use Ctrl+z or start with a `&` at the end of the `kubectl port-forward` command

```bash
# create a pod that runs on a specific port
kubectl run portpod --image=nginx:latest --port=80

# setup port forwarding, on a local port that maps to Pod port
kubectl port-forward portpod 1234:80 &

# from local machine, where you ran port-forward from, access the pod on port you set
curl localhost:1234
```