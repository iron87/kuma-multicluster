#1 Deploy dataplane ok GKE autopilot
```
kubectl apply -f demo.yaml
```
```
 Warning  FailedCreate  2m11s (x16 over 4m56s)  replicaset-controller  Error creating: admission webhook "gkepolicy.common-webhooks.networking.gke.io" denied the request: GKE Policy Controller rejected the request because it violates one or more policies: {"[denied by autogke-default-linux-capabilities]":["linux capability 'NET_ADMIN' on container 'kuma-init' not allowed; Autopilot only allows the capabilities: 'AUDIT_WRITE,CHOWN,DAC_OVERRIDE,FOWNER,FSETID,KILL,MKNOD,NET_BIND_SERVICE,NET_RAW,SETFCAP,SETGID,SETPCAP,SETUID,SYS_CHROOT,SYS_PTRACE'. Requested by user: 'system:serviceaccount:kube-system:replicaset-controller', groups: 'system:serviceaccounts,system:serviceaccounts:kube-system,system:authenticated'."]}

```


#2 Deploy on Openstack clusters

- Il cloud controller crea un solo load balancer (zone-ingress) anzichè uno per ogni zona. Il problema potrebbe essere la naming convention usata dal cloud controller per creare i nomi.

```
 
```

