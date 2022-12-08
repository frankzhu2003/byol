# byol

# Central Control Plane for Observability
<p align="center"><img src="/image/fluentd.png" width="40%" alt="Fluentd Logo" /></p>

<p align="center"><img src="/image/loki.png" width="40%" alt="Grafana Logo" /></p>

## Prerequisite
The following tools need to be install on your machine :
- K8s
- Chronosphere Collector (optional)



### 1. Install the ngninx ingress controller
```
helm repo add nginx-stable https://helm.nginx.com/stable
helm install ngninx nginx-stable/nginx-ingress --set controller.enableLatencyMetrics=true --set prometheus.create=true --set controller.config.name=nginx-config
```
this command will install the nginx controller with configmap named nginx-config

To generate logs containing extended information related to our ingress controller we need to update the configuration of ngninx to explose more logs.
let's modify the configmap to enhance the login format
```
kubectl edit cm nginx-config
```
add the following line:
```yaml
data:
  log-format: $remote_addr [$time_local] "$request" $status $body_bytes_sent $request_time $upstream_addr $upstream_response_time $proxy_host $upstream_status $resource_name $resource_type $resource_namespace $service
```

### 2. Install the Prometheus Operator
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install [RELEASE_NAME] prometheus-community/kube-prometheus-stack
```

### 3. Install the Spring Petclinic application
```
kubectl apply -f petclinic/petclinic.yaml    
kubectl apply -f petclinic/ingress_petclinic.yaml
```
Deploy the servicemonitor:
```
kubectl apply -f prometheus/servicemonitor_ingress.yaml    
```

### 4. Install Loki
#### Install Loki with Promtail
```
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm upgrade --install loki grafana/loki-stack
```

### 5. Install the Fluentd
```
kubectl apply -f fluentd/service_account.yaml    
kubectl apply -f fluentd/fluentd-manifest.yaml
```
Deploy the servicemonitor:
```
kubectl apply -f prometheus/servicemonitor_fluentd.yaml    
```

