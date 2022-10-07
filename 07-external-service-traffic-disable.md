
On the control plane cluster, edit the default mesh

```
kubectl edit meshes.kuma.io default
```

and add the networking config 

```
spec:
  networking:
    outbound:
      passthrough: false" | kubectl apply -f -
```

then define an external service

```
apiVersion: kuma.io/v1alpha1
kind: ExternalService
mesh: default
metadata:
  name: redis-vm-ext
spec:
  tags:
    kuma.io/service: redis-vm
    kuma.io/protocol: tcp
  networking:
    address: 10.154.16.54:6379
    tls:
      enabled: false
---
apiVersion: kuma.io/v1alpha1
kind: TrafficPermission
mesh: default
metadata:
  name: demoapp-to-redis-vm
spec:
  sources:
    - match:
        kuma.io/service: demo-app_kuma-demo_svc_5000
  destinations:
    - match:
        kuma.io/service: redis-vm
```