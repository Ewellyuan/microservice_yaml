### Create Dockerfile && Build images
docker build -t [service_name_version] .

### Step 1.
kubectl apply -f <(istioctl kube-inject -f microservice_yaml/kube_services.yaml)

### Step 2.
kubectl apply -f microservice_yaml/istio_gateway.yaml

### Step 3.
kubectl apply -f microservice_yaml/istio_destination.yaml

### Step 4.
kubectl get all
kubectl describe [pod/xxx]

### Step 5.
kubectl apply -f <(istioctl kube-inject -f microservice_yaml/kube_service_string.yaml)

### Step 6.
kubectl apply -f microservice_yaml/istio_destination_service_string.yaml

### Step 7.
kubectl scale deployment service-string-v1 --replicas 3
kubectl scale deployment service-string-v1 --replicas 1

### Step 8.
apply virtual service

### Step 9.
### Jaeger dashboard
### http://localhost:16686/search
kubectl port-forward -n istio-system $(kubectl get pod -n istio-system -l app=jaeger -o jsonpath='{.items[0].metadata.name}') 16686:16686 &

### prometheus
### http://localhost:9090/graph
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=prometheus -o jsonpath='{.items[0].metadata.name}') 9090:9090 &

### grafana
### http://localhost:3000/?orgId=1
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000 &

### servicegraph
### http://localhost:8088/force/forcegraph.html
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=servicegraph -o jsonpath='{.items[0].metadata.name}') 8088:8088 &


