# Cert-manager

Create the secret to store the api token for cloudflare.

kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.18.2/cert-manager.yaml

```sh
kubectl create secret generic cloudflare-api-token \
  --namespace=cert-manager \
  --from-literal=api-token=<your-cloudflare-api-token>
```
