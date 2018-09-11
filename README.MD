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
virtual service

### Step 9.
