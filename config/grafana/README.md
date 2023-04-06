
```
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```

```
helm template --values helm-values.yaml grafana --include-crds -n grafana grafana/grafana --version 6.52.7 > grafana.yaml 
```