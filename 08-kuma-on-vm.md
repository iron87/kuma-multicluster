Run the control plane

```
KUMA_MODE=zone \
 KUMA_MULTIZONE_ZONE_NAME=zone-3 \
 KUMA_MULTIZONE_ZONE_GLOBAL_ADDRESS=grpcs://10.154.16.52:5685 \
 ./kuma-cp run
```

Generate an ingress token

```
  ./kumactl generate zone-ingress-token --zone=zone-3 > /tmp/ingress-token
```

```
echo "type: ZoneIngress
 name: ingress-01
 networking:
   address: 10.154.16.50 # address that is routable within the zone
   port: 10001
   advertisedAddress: 10.154.16.50 # an address which other zones can use to consume this zone-ingress
   advertisedPort: 10001 # a port which other zones can use to consume this zone-ingress" > ingress-dp.yaml
```


cp-address is the ip ov the vm where kuma runs

```
kuma-dp run --proxy-type=ingress --cp-address=https://localhost:5678 --dataplane-token-file=/tmp/ingress-token --dataplane-file=ingress-dp.yaml
```

---
install redis
```
sudo apt install redis-server
```
edit redis config file to run redis on different port (26379)

generate redistoken
```
./kumactl generate dataplane-token --name=redis > kuma-token-redis
```

Then run the dataplane process

```
kuma-dp run \
   --cp-address=https://localhost:5678/ \
   --dns-enabled=false \
   --dataplane-token-file=kuma-token-redis \
   --dataplane="type: Dataplane
mesh: default
name: redis
networking:
  address: 10.154.16.50 # Or any public address (needs to be reachable by every other dp in the zone) 
  inbound:
    - port: 16379
      servicePort: 26379
      tags:
        kuma.io/service: redis_kuma-demo_svc_6379
        kuma.io/protocol: tcp
  admin:
    port: 9902"
```