
# Demo App 

## App deploy on k8s

For all zone clusters

```
git clone https://github.com/kumahq/kuma-counter-demo.git
```

For every zone cluster

```
kubectl apply -f demo.yaml
```


## Kong Ingress 

install kong: https://github.com/kumahq/kuma-counter-demo/blob/master/kong.yaml

Create an ingress for demo app

```
echo "apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: demo-ingress
  namespace: kuma-demo
  annotations:
    konghq.com/strip-path: 'true'
spec:
  ingressClassName: kong
  rules:
  - http:
      paths:
      - path: /
        pathType: ImplementationSpecific
        backend:
          service:
            name: demo-app
            port: 
              number: 5000" | kubectl apply -f -
```