

create a secret

```
kubectl create secret generic john-basicauth --from-literal=kongCredType=basic-auth    --from-literal=username=johndoe --from-literal=password=foo -n kuma-demo
```

then create a Kong Plugin resource and a KongConsumer

```
apiVersion: configuration.konghq.com/v1
kind: KongPlugin
metadata:
  name: basic-auth-example
  namespace: kuma-demo
config:
  hide_credentials: true
plugin: basic-auth
---
apiVersion: configuration.konghq.com/v1
kind: KongConsumer
metadata:
  annotations:
    kubernetes.io/ingress.class: kong
  name: johndoe
  namespace: kuma-demo
username: johndoe
credentials:
- john-basicauth
```

then edit the demo-app Service and add the following annotations 
```
konghq.com/plugins: basic-auth-example
```


johndoe foo

