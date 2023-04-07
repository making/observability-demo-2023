# Observability Demo

## Create a kind cluster
```
$ kind version
kind v0.18.0 go1.20.3 darwin/amd64
```

```
kind create cluster --config config/kind/kind-expose-port.yaml --image kindest/node:v1.25.8
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
kubectl apply -f config/prometheus/rendered/prometheus.yaml -n prometheus
```

* URL: https://prometheus.127-0-0-1.sslip.io


```
kubectl create ns loki
kubectl apply -f config/grafana-loki/rendered/loki.yaml -n loki
```

```
kubectl create ns tempo
kubectl apply -f config/grafana-tempo/rendered/tempo.yaml -n tempo
```

```
kubectl create ns grafana
kubectl apply -f config/grafana/rendered/grafana.yaml -n grafana
```

* URL: https://grafana.127-0-0-1.sslip.io
* username: `grafana`
* password: `grafana`

## Deploy Grafana Agent

```
kubectl create ns grafana-agent
kubectl apply -f config/grafana-agent/rendered/grafana-agent.yaml
```

## Deploy vehicle-api

```
kubectl create ns vehicle
```

```
kubectl apply -f config/vehicle-api -n vehicle
```

* URL: https://vehicle-api.127-0-0-1.sslip.io/swagger-ui/index.html


Perform a load test

```
echo "GET https://vehicle-api.127-0-0-1.sslip.io/vehicles" | vegeta attack  -insecure -duration=180s -rate 10 | tee results.bin | vegeta report
```

