## Kumactl installation

install kumactl
```
curl -L https://kuma.io/installer.sh | sh -
```
add kumactl to your path
```
cd kuma-1.8.0/bin
PATH=$(pwd):$PATH
```

## Control Plane installation
```
kumactl install control-plane --mode=global | kubectl apply -f -
```

Find the external ip of the remote sync service

```
kubectl get services -n kuma-system
```
You will use it (param <global-kds-address>) to setup the zone control planes

## Enable mutual tls

This step is mandatory to enable cross zone communication

```
echo "apiVersion: kuma.io/v1alpha1
kind: Mesh
metadata:
  name: default
spec:
  mtls:
    enabledBackend: ca-1
    backends:
      - name: ca-1
        type: builtin
        dpCert:
          rotation:
            expiration: 1d
        conf:
          caCert:
            RSAbits: 2048
            expiration: 10y" | kubectl apply -f -
```


## (Optional) Expose the kuma gui on internet


install kong: https://github.com/kumahq/kuma-counter-demo/blob/master/kong.yaml


add an ingress rule for kuma control plane

```
echo "apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: kuma-ingress
  namespace: kuma-system
  annotations:
    konghq.com/strip-path: 'true'
spec:
  ingressClassName: kong
  rules:
  - http:
      paths:
      - path: /gui
        pathType: ImplementationSpecific
        backend:
          service:
            name: kuma-control-plane
            port: 
              number: 5681" | kubectl apply -f -
```
### References

- https://kuma.io/docs/1.8.x/deployments/multi-zone/#usage
- https://kuma.io/docs/1.8.x/policies/mutual-tls/#mutual-tls
