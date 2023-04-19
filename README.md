# Observability Demo

<img width="1024" alt="image" src="https://user-images.githubusercontent.com/106908/232938658-a8028257-225a-4031-b3fc-a0a41df5286a.png">

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
kubectl wait --for=condition=Available=true deploy/cert-manager-webhook -n cert-manager --timeout=60s
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
kubectl wait --for=condition=Available=true deploy/prometheus-server -n prometheus --timeout=60s
```

* URL: https://prometheus.127-0-0-1.sslip.io
* username: `prometheus`
* password: `prometheus`

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
kubectl wait --for=condition=Available=true deploy/grafana -n grafana --timeout=60s
```

* URL: https://grafana.127-0-0-1.sslip.io
* username: `grafana`
* password: `grafana`

Metrics not sent yet

## Deploy Grafana Agent

```
kubectl create ns grafana-agent
kubectl apply -f config/grafana-agent/rendered/grafana-agent.yaml
```

## Deploy [vehicle-api](https://github.com/making/vehicle-api) & [vehicle-ui](https://github.com/making/vehicle-ui)

```
kubectl create ns vehicle
```

```
kubectl apply -f config/vehicle-api -n vehicle
kubectl wait --for=condition=Available=true deploy/vehicle-api -n vehicle --timeout=60s
```

* URL: https://vehicle-api.127-0-0-1.sslip.io/swagger-ui/index.html

```
kubectl apply -f config/vehicle-ui -n vehicle
kubectl wait --for=condition=Available=true deploy/vehicle-ui -n vehicle --timeout=60s
```

* URL: https://vehicle-ui.127-0-0-1.sslip.io


Perform a load test

```
echo "GET https://vehicle-ui.127-0-0-1.sslip.io?name=" | vegeta attack  -insecure -duration=180s | tee results.bin | vegeta report
```

```
curl -H "Accept: application/openmetrics-text" -sk https://vehicle-api.127-0-0-1.sslip.io//actuator/prometheus
```

https://grafana.127-0-0-1.sslip.io/d/spring-boot-dashboard/spring-boot-dashboard?orgId=1&var-cluster=kind&var-namespace=vehicle&var-application=vehicle-api&&var-hikaricp=HikariPool-1&viewPanel=116&from=now-15m&to=now


## How to delete the installation

```
kubectl delete -f config/vehicle-ui -n vehicle
kubectl delete -f config/vehicle-api -n vehicle
kubectl delete -f config/grafana-agent/rendered/grafana-agent.yaml
kubectl delete -f config/grafana/rendered/grafana.yaml -n grafana
kubectl delete -f config/grafana-tempo/rendered/tempo.yaml -n tempo
kubectl delete -f config/grafana-loki/rendered/loki.yaml -n loki
kubectl delete -f config/prometheus/rendered/prometheus.yaml -n prometheus
kubectl delete -f https://projectcontour.io/quickstart/contour.yaml
kubectl delete -f https://github.com/cert-manager/cert-manager/releases/download/v1.11.0/cert-manager.yaml
```

```
kind delete cluster
```