```
ytt -f grafana-agent.ytt.yaml -f agent.yaml  --file-mark 'agent.yaml:type=data' > grafana-agent.yaml
```