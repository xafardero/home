helm repo add grafana https://grafana.github.io/helm-charts

helm upgrade --install --values loki.yaml loki grafana/loki-stack -n monitoring