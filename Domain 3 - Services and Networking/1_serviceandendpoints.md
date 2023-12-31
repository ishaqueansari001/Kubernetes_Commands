#### Documentation Referred:

https://kubernetes.io/docs/concepts/services-networking/service/

#### Step 1: Creating Backend PODS
```sh
kubectl run backend-pod-1 --image=nginx
kubectl run backend-pod-2 --image=nginx
```
#### Step 2: Creating Frontend PODS
```sh
kubectl run frontend-pod --image=ubuntu --command -- sleep 3600
```
#### Step 3: Test the Connection between Frontend and Backend PODs
```sh
kubectl get pods -o wide
kubectl exec -it frontend-pod -- bash
apt-get update && apt-get -y install curl
curl <BACKEND-POD-1-IP>
```
#### Step 4: Create a new Service

```sh
 nano service.yaml
```
```sh
apiVersion: v1
kind: Service
metadata:
   name: ansari-service
spec:
   ports:
   - port: 8080
     targetPort: 80
```
```
kubectl apply -f service.yaml
kubectl get service
kubectl describe service ansari-service
**Before Creating Endpoint lets check connectivity to Service**
kubectl exec -it frontend-pod -- bash
curl <SERVICE-IP:8080>
```
#### Step 5: Associate Endpoints with Service
```sh
nano endpoint.yaml
```
```sh
apiVersion: v1
kind: Endpoints
metadata:
  name: ansari-service
subsets:
  - addresses:
      - ip: 10.244.0.23
    ports:
      - port: 80
```
```sh
kubectl apply -f endpoint.yaml
```
#### Step 6: Test the Connection
```sh
kubectl describe service ansari-service
kubectl exec -it frontend-pod -- bash
curl <SERVICE-IP:8080>
```

#### Step 7: Delete the Created Resources
```sh
kubectl delete service ansari-service
kubectl delete endpoints ansari-service
kubectl delete pod backend-pod-1
kubectl delete pod backend-pod-2
kubectl delete pod frontend-pod
```
