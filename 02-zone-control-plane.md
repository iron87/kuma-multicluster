## Control plane installation

For every zone: (ensure you're using the proper KUBECONFIG)

```
kumactl install control-plane \
--mode=zone \
--zone=<zone name> \
--ingress-enabled \
--kds-global-address grpcs://<global-kds-address>:5685 | kubectl apply -f -
```
example
```
    kumactl install control-plane \
    --mode=zone \
    --zone=zone-1 \
    --ingress-enabled \
    --kds-global-address grpcs://10.154.16.52:5685 | kubectl apply -f -
```

### References

- https://kuma.io/docs/1.8.x/deployments/multi-zone/#usage

