# Observability Demo

## Create a kind cluster
```
$ kind version
kind v0.18.0 go1.20.3 darwin/amd64
```

```
kind create cluster --config config/kind/kind-expose-port.yaml --image kindest/node:v1.25.8@sha256:00d3f5314cc35327706776e95b2f8e504198ce59ac545d0200a89e69fce10b7f
```

## Install cert-manager

```
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.11.0/cert-manager.yaml
```

```
kubectl apply -f config/cert-manager/clusterissuer.yaml
```

## Install Contour (Ingress Controller)

```
kubectl apply -f https://projectcontour.io/quickstart/contour.yaml
```

## Deploy Prometheus, Loki, Tempo, Grafana

```
kubectl create ns prometheus
kubectl apply -f config/prometheus/prometheus.yaml -n prometheus
```

```
kubectl create ns loki
kubectl apply -f config/grafana-loki/loki.yaml -n loki
```

```
kubectl create ns tempo
kubectl apply -f config/grafana-tempo/tempo.yaml -n tempo
```

```
kubectl create ns grafana
kubectl apply -f config/grafana/grafana.yaml -n grafana
```

## Deploy Grafana Agent

```
kubectl create ns grafana-agent
kubectl apply -f config/grafana-agent/grafana-agent.yaml
```

## Deploy vehicle-api

```
kubectl create ns vehicle
```

```
kubectl apply -f config/vehicle-api -n vehicle
```

