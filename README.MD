### Create Dockerfile && Build images
docker build -t service .

### Step 1.
kubectl apply -f <(istioctl kube-inject -f kube_services.yaml)

### Step 2.
kubectl apply -f mykube/istio_gateway.yaml

### Step 3.
kubectl apply -f mykube/istio_destination.yaml

### Step 4.
kubectl get all
kubectl describe pod/callsvc-v1-755ccb5c87-gbr4d

### Step 5.
kubectl apply -f <(istioctl kube-inject -f kube_string_service.yaml)

### Step 6.
kubectl apply -f mykube/istio_string_service_destination.yaml

### Step 7.
kubectl scale deployment string-service-v1 --replicas 3
kubectl scale deployment string-service-v1 --replicas 1

### Step 8.
