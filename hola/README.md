# Hola - Hello World Application

Simple hello world web application deployed at hola.xaf.world.

## Deployment

Apply all manifests:
```bash
kubectl apply -f namespace.yml
kubectl apply -f deployment.yml
kubectl apply -f service.yml
kubectl apply -f ingress.yml
```

Or apply the entire directory:
```bash
kubectl apply -f hola/
```

## Verify

Check deployment status:
```bash
kubectl -n hola get all
kubectl -n hola get ingress
kubectl -n hola get certificate
```

Watch certificate issuance:
```bash
kubectl -n hola get certificate,orders,challenges -w
```

## Access

Once deployed and DNS is configured:
- https://hola.xaf.world

## Components

- **Namespace**: `hola`
- **Deployment**: `hola-deployment` (hashicorp/http-echo container)
- **Service**: `hola-service` (ClusterIP on port 80)
- **Ingress**: `hola-ingress` (Traefik with Let's Encrypt TLS)
