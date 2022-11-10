## Default traffic permission

Default TrafficPermission policy allow all the connections, so it needs to be removed

```
kubectl delete trafficpermissions-kuma-io allow-all-default
```

Then apply the new policies:

```
apiVersion: kuma.io/v1alpha1
kind: TrafficPermission
mesh: default
metadata:
  name: demoapp-to-redis
spec:
  sources:
    - match:
        kuma.io/service: demo-app_kuma-demo_svc_5000
  destinations:
    - match:
        kuma.io/service: redis_kuma-demo_svc_6379
---
apiVersion: kuma.io/v1alpha1
kind: TrafficPermission
mesh: default
metadata:
  name: all-to-demoapp
spec:
  sources:
    - match:
        kuma.io/service: '*'
  destinations:
    - match:
        kuma.io/service: demo-app_kuma-demo_svc_5000
```




### References

- https://kuma.io/docs/1.8.x/policies/traffic-permissions/#usage


