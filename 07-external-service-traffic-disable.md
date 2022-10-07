
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
type: ExternalService
mesh: default
name: redis-vm
tags:
  kuma.io/service: redis-vm
  kuma.io/protocol: tcp
networking:
  address: 10.154.16.54:6379
  tls:
    enabled: false
```