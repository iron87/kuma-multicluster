## Expose jaeger api on global cluster

Install the builtin observability (kumactl install observability or kumactl install tracing or kumactl install observability --components jaeger,zipkin)

Then create an ingress for the jaeger service, in order to send data from zones

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: jaeger-ingress
  namespace: mesh-observability
  annotations:
    konghq.com/strip-path: 'true'
spec:
  ingressClassName: kong
  rules:
  - http:
      paths:
      - path: /jaeger
        pathType: ImplementationSpecific
        backend:
          service:
            name: jaeger-collector
            port: 
              number: 9411"
```

## Zones configuration

You can add a Tracing backend to the mesh and use the Jaeger endpoint exposed on global cluster

```
apiVersion: kuma.io/v1alpha1
kind: Mesh
metadata:
  name: default
spec:
  apiVersion: kuma.io/v1alpha1
  tracing:
    backends:
    - conf:
        url: http://<global-ingress-controller-ip:80/jaeger/api/v2/spans # don't remove the port
      name: jaeger-collector
      sampling: 100
      type: zipkin
    defaultBackend: jaeger-collector

```

### Add a Traffic Trace resorce 
```
apiVersion: kuma.io/v1alpha1
kind: TrafficTrace
mesh: default
metadata:
  name: trace-all-traffic
spec:
  selectors:
  - match:
      kuma.io/service: '*'
  conf:
    backend: jaeger-collector # or the name of any backend defined for the mesh 
```
