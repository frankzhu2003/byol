# byol

## Prerequisite
The following tools need to be install on your machine :
- K8s
- Chronosphere Collector (optional)



###  Install the ngninx controller
```
helm repo add nginx-stable https://helm.nginx.com/stable
helm install ngninx nginx-stable/nginx-ingress --set controller.enableLatencyMetrics=true --set prometheus.create=true --set controller.config.name=nginx-config
```
this command will install the nginx controller with configmap named nginx-config

###  Install the Prometheus Operator
```
helm repo add nginx-stable https://helm.nginx.com/stable
helm install ngninx nginx-stable/nginx-ingress --set controller.enableLatencyMetrics=true --set prometheus.create=true --set controller.config.name=nginx-config
```
this command will install the nginx controller with configmap named nginx-config
