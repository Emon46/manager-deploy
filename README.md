let's deploy a local `kind cluster`:
``` 
kind create cluster
```

deploy the `prometheus` with persistent volume: 
``` 
helm install prometheus prometheus-community/kube-prometheus-stack -n monitoring  --create-namespace --values=prometheus/values.yaml
```
deploy `loki` with persistent volume
``` 
helm install  --values values.yaml  loki grafana/loki-stack -n loki --create-namespace
```
deploy vector with to have `loki` and `prometheus` as sink 
``` 
helm install vector vector/vector --namespace vector --create-namespace --values=vector/values.yaml
```

now create a service monitor to export metrics from vector prometheus sink to prometheus exporter.
``` 
kubectl apply -f vector/service-monitor.yaml
```