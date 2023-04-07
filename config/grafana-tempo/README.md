
```
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```

```
helm template --values helm-values.yaml tempo --include-crds -n tempo grafana/tempo --version 1.0.3 > rendered/tempo.yaml 
```