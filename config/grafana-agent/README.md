```
ytt -f grafana-agent.ytt.yaml -f agent.yaml  --file-mark 'agent.yaml:type=data' > rendered/grafana-agent.yaml
```