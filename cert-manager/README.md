# Cert-manager

Create the secret to store the api token for cloudflare.

```sh
kubectl create secret generic cloudflare-api-token \
  --namespace=cert-manager \
  --from-literal=api-token=<your-cloudflare-api-token>
```
