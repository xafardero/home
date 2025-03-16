# 📦 Basic Kubernetes dashboard

We use the https://github.com/kubernetes/dashboard/tree/master repo.

Installation:
```sh
# Add kubernetes-dashboard repository
helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/
# Deploy a Helm Release named "kubernetes-dashboard" using the kubernetes-dashboard chart
helm upgrade --install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard --create-namespace --namespace kubernetes-dashboard
```


Port fordward:
```sh
kubectl -n kubernetes-dashboard port-forward svc/kubernetes-dashboard-kong-proxy 8443:443
```

Generate token:
```sh
kubectl -n kubernetes-dashboard create token admin-user
```


To access to the dashboard with this link https://localhost:8443
