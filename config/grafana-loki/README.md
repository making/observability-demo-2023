https://grafana.com/docs/loki/latest/installation/helm/install-monolithic/

```
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```

```
helm template --values helm-values.yaml loki --include-crds -n loki grafana/loki --version 5.0.0 > loki.yaml 
```