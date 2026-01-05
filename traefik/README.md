🔧 Step 5: Deploy Traefik ingress

Install Traefik via Helm (you can also use k3s’ default, but Helm gives more control):

helm repo add traefik https://traefik.github.io/charts
helm repo update

helm install traefik traefik/traefik \
  --namespace kube-system \
  --set service.type=LoadBalancer


This will create a LoadBalancer service → MetalLB will assign an external IP (from the pool).

Check:

kubectl get svc -n kube-system traefik
