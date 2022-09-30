kumactl install control-plane \
--mode=zone \
--zone=<zone name> \
--ingress-enabled \
--kds-global-address grpcs://<global-kds-address>:5685 | kubectl apply -f -

example

kumactl install control-plane \
--mode=zone \
--zone=zone-2 \
--ingress-enabled \
--kds-global-address grpcs://34.78.184.110:5685 | kubectl apply -f -



