
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

```
helm template --values helm-values.yaml prometheus --include-crds -n prometheus prometheus-community/prometheus --version 20.2.0 | ytt -f- -f overlay.yaml > rendered/prometheus.yaml 
```