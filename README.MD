### Create Dockerfile && Build images
docker build -t go_sumsvc_v1 .
docker build -t go_sumsvc_v2 .
docker build -t go_callsvc_v1 .

### Step 1.
kubectl apply -f <(istioctl kube-inject -f mykube/kube_services.yaml)

### Step 2.
kubectl apply -f mykube/istio_gateway.yaml

### Step 3.
kubectl apply -f mykube/istio_destination.yaml

### Step 4.
kubectl get all
kubectl describe pod/callsvc-v1-755ccb5c87-gbr4d