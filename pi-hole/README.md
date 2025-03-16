# 🫓 Pi-hole

Create folders in the raspberry
``` bash
sudo mkdir -p /var/pihole/dnsmasq
sudo mkdir -p /var/pihole/etc
```

To create the secret with the password for the admin web interface
``` bash
kubectl -n hole create secret generic pi-hole --from-file=admin-web.txt
```

To get the password for the admin web interface
```bash
kubectl get secret -n hole pi-hole -o jsonpath="{.data.admin-web\.txt}" | base64 --decode
```