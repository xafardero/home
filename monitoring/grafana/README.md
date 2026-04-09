# Kubernetes Grafana

This directory contains Kubernetes manifests to deploy Grafana in the `monitoring` namespace, provision a Prometheus data source, expose Grafana internally with a `Service`, and publish it externally with `Ingress` + TLS.

## Files

- `deployment.yaml`: Grafana `Deployment` (`grafana/grafana:12.4.2`) with:
	- container port `3000`
	- resource requests/limits
	- ephemeral storage (`emptyDir`) at `/var/lib/grafana`
	- datasource provisioning mount at `/etc/grafana/provisioning/datasources`
- `grafana-datasource-config.yaml`: `ConfigMap` named `grafana-datasources` that provisions a Prometheus datasource.
- `service.yaml`: `NodePort` `Service` on port `3000`.
- `ingress.yaml`: `Ingress` for `grafana.xafardero.us` using class `traefik` and TLS secret `grafana-tls`.

## Prerequisites

- A Kubernetes cluster.
- Namespace `monitoring` exists.
- Ingress controller handling class `traefik`.
- cert-manager installed and configured with:
	- ClusterIssuer `letsencrypt-prod`
	- DNS01 provider integration matching your environment (current annotations reference Cloudflare).
- A reachable Prometheus endpoint at `http://prometheus-service.monitoring.svc:8080`.

## Deploy

Apply resources in this order so the `ConfigMap` exists before the `Deployment` mounts it:

```bash
kubectl apply -f grafana-datasource-config.yaml
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f ingress.yaml
```

Or apply all manifests at once:

```bash
kubectl apply -f .
```

## Verify

```bash
kubectl -n monitoring get deploy,po,svc,ingress
kubectl -n monitoring describe ingress ingress-grafana
```

When DNS and TLS are ready, Grafana should be available at:

- `https://grafana.xafardero.us`

## Customize

- Change Grafana version in `deployment.yaml` (`image: grafana/grafana:...`).
- Update datasource URL in `grafana-datasource-config.yaml`.
- Change hostname and TLS secret in `ingress.yaml`.
- If persistent dashboards/users are required, replace `emptyDir` storage with a persistent volume claim.

