# ArgoCD

Installation of ArgoCD:

```shell
#Create namespace:

kubectl apply -f namespace.yaml

#Install ArgoCD:
kubectl apply -n argocd -f argocd/install.yml


#Install Ingress:
kubectl apply  -f argocd/ingress.yml

#Disable tls to avoid to configure it :sweat_face:
kubectl apply -f argocd/config_map.yml
```


Get admin password:
```shell
kubectl --namespace argocd get secret argocd-initial-admin-secret -o json | jq -r '.data.password' | base64 -d
```