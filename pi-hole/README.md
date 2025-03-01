Create folders in the raspberry
``` bash
sudo mkdir -p /var/pihole/dnsmasq
sudo mkdir -p /var/pihole/etc
```

``` bash
kubectl -n hole create secret generic pi-hole --from-file=admin-web.txt
```
