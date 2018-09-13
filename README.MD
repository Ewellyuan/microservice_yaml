### 宿主环境
* OSX 10.13.6 (17G65)
* Kubernetes Server Version: version.Info{Major:"1", Minor:"10", GitVersion:"v1.10.3", GitCommit:"2bba0127d85d5a46ab4b778548be28623b32d0b0", GitTreeState:"clean", BuildDate:"2018-05-21T09:05:37Z", GoVersion:"go1.9.3", Compiler:"gc", Platform:"linux/amd64"}
* Istio v1.0
* Node v10.0
* Go version go1.10.3 darwin/amd64

### Create Dockerfile && Build images
```
docker build -t [service_name_version] .
```

### Step 1. 通过istio注入方式，部署服务
```
kubectl apply -f <(istioctl kube-inject -f microservice_yaml/kube_services.yaml)
```

### Step 2. 部署 gateway
```
kubectl apply -f microservice_yaml/istio_gateway.yaml
```

### Step 3. 部署 destination
```
kubectl apply -f microservice_yaml/istio_destination.yaml
```

### Step 4. 检查部署情况：pod、service、deployment
```
kubectl get all
kubectl describe [pod/xxx]
```

### Step 5. 新增服务
```
kubectl apply -f <(istioctl kube-inject -f microservice_yaml/kube_service_string.yaml)
```

### Step 6. 为新服务部署 destination
```
kubectl apply -f microservice_yaml/istio_destination_service_string.yaml
```

### Step 7. 弹性伸缩某个服务
```
kubectl scale deployment service-string-v1 --replicas 3
kubectl scale deployment service-string-v1 --replicas 1
```

### Step 8. 流量控制：按照比例分发流量、独占流量
```
kubectl apply -f microservice_yaml/istio_virtualservice_math_25_75.yaml
kubectl apply -f microservice_yaml/istio_virtualservice_math_v1.yaml
kubectl apply -f microservice_yaml/istio_virtualservice_math_v2.yaml
```

### Step 9. 周边服务
### Jaeger dashboard
### http://localhost:16686/search
```
kubectl port-forward -n istio-system $(kubectl get pod -n istio-system -l app=jaeger -o jsonpath='{.items[0].metadata.name}') 16686:16686 &
```

### prometheus
### http://localhost:9090/graph
```
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=prometheus -o jsonpath='{.items[0].metadata.name}') 9090:9090 &
```

### grafana
### http://localhost:3000/?orgId=1
```
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000 &
```

### servicegraph
### http://localhost:8088/force/forcegraph.html
```
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=servicegraph -o jsonpath='{.items[0].metadata.name}') 8088:8088 &
```

### Step 10. 删除服务以及 destination
```
kubectl delete -f microservice_yaml/kube_service_string.yaml
kubectl delete -f microservice_yaml/istio_destination_service_string.yaml
```