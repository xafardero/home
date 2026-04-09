# Cert Manager

Manages TLS certificates via [Let's Encrypt](https://letsencrypt.org/) using the [Cloudflare DNS-01 challenge](https://cert-manager.io/docs/configuration/acme/dns01/cloudflare/). Two `ClusterIssuer` resources are provided: one for staging (testing) and one for production.

## Installation

### 1. Install cert-manager

```sh
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.20.0/cert-manager.yaml
```

### 2. Create the Cloudflare API token secret

```sh
kubectl create secret generic cloudflare-api-token \
  --namespace=cert-manager \
  --from-literal=api-token=<YOUR_CLOUDFLARE_API_TOKEN>
```

### 3. Apply the ClusterIssuers

```sh
kubectl apply -f issuer.yml
```

Verify both issuers are ready:

```sh
kubectl get clusterissuer
```

## Usage

Reference the appropriate issuer in your `Certificate` or `Ingress` resources:

| Issuer name | Purpose |
|-------------|---------|
| `letsencrypt-staging` | Testing — issues untrusted certificates, not subject to rate limits |
| `letsencrypt-prod` | Production — issues trusted certificates |

Use `letsencrypt-staging` first to confirm DNS and ACME configuration is correct before switching to `letsencrypt-prod`.
