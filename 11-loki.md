## Expose loki api on global
```yaml
echo "apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: loki-ingress
  namespace: mesh-observability
  annotations:
    konghq.com/strip-path: 'true'
spec:
  ingressClassName: kong
  rules:
  - http:
      paths:
      - path: /loki
        pathType: ImplementationSpecific
        backend:
          service:
            name: loki
            port: 
              number: 3100" | kubectl apply -f -
```

On zones' observability manifest you have to modify the loki-promtail configuration, by setting the value of: 
```
-client.url=http://loki.mesh-observability:3100/loki/api/v1/push 
```
to 
```
-client.url=http://<global-ingress-controller-ip>/loki/loki/api/v1/push
```

Then you can add a Traffig Log Policy to your mesh to start emitting access logs

```
apiVersion: kuma.io/v1alpha1
kind: Mesh
metadata:
  name: default
spec:
  logging:
    defaultBackend: stdout
    backends:
      - name: stdout
        type: file
        conf:
          path: /dev/stdout
```
