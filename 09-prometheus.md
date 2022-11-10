# Prometheus global

```bash
kubectl create namespace prometheus

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

helm repo update

helm upgrade -i -n prometheus --set server.persistentVolume.enabled=false --set server.extraFlags={web.enable-remote-write-receiver}  prometheus prometheus-community/prometheus
```

then expose Prometheus servivce in order to call thw write api from the zone clusters

```yaml
echo "apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: prometheus-ingress
  namespace: prometheus
spec:
  ingressClassName: kong
  rules:
  - http:
      paths:
      - path: /prometheus
        pathType: ImplementationSpecific
        backend:
          service:
            name: prometheus-server
            port: 
              number: 80" | kubectl apply -f -
```

Get the ingress ip 


# Prometheus zone

```
kubectl create namespace prometheus

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

helm repo update
```
We can't use dynamic volume provisioning on our clusters, then I disable the persistentVolume on prometheus deployment

```
helm upgrade -i -n prometheus --set-file extraScrapeConfigs=../prometheus/scrape-configs.yaml --set-file server.remote_write=../prometheus/remote-write.yaml  --set server.persistentVolume.enabled=false  prometheus prometheus-community/prometheus
```

Update the mesh config in order to enable prometheus

```yaml
apiVersion: kuma.io/v1alpha1
kind: Mesh
metadata:
  name: default
spec:
  metrics:
    enabledBackend: prometheus-1
    backends:
    - name: prometheus-1
      type: prometheus
      conf:
        skipMTLS: false
```


 install grafana

 kumactl install observability --components  grafana > grafana.yaml


echo "apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: grafana-ingress
  namespace: kong
spec:
  ingressClassName: kong
  rules:
  - http:
      paths:
      - path: /grafana
        pathType: ImplementationSpecific
        backend:
          service:
            name: grafana
            port: 
              number: 80" | kubectl apply -f -



https://github.com/kumahq/kuma-grafana-datasource


    




