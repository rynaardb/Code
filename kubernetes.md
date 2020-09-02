# Kubernetes

### [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/) (run commands against Kubernetes clusters)

```bash
# Get info on pods in use. Has info on why they failed if they did.
kubectl describe pods

# Get services across all namespaces
kubectl get svc --all-namespaces

# Port forward the <pod> from 5432 to localhost 5300 port
kubectl port-forward <pod> 5300:5432
```