1. Create Cluster
```sh
kind create cluster
```
2. Make sure a docker container is running.
3. Create Namespace
```sh
kubectl create namespace demo
```
4. create deployment.
```sh
kubectl --namespace demo create deployment webproject --image=nginx --replicas 3
```
5. check what pods are running
```sh
kubectl --namespace demo get pods
```
Should return 3 deployments with name webproject are running.
6. Scaling up and down
```sh
kubectl --namespace demo scale deployment webproject --replicas 1
```
7. Get logs from the pod
```sh
kubectl --namespace demo logs webproject-7823hkj234hl23b4l
```
8. Get all namespaces
```sh
kubectl get namespace
```
9. Delete namespaces
```sh
kubectl delete namespace <Name>
```

# Automating it using yaml
![[Pasted image 20250111192222.png]]
![[Pasted image 20250111192318.png]]
the bottom one is the service we are creating

10. USING manifest.yaml file to interact with the cluster
```sh
kubectl --namespace rasa apply -f manifest.yaml
```
11. Get services from a namespace
```sh
kubectl --namespace rasa get svc
```
will give the details with ips of the services
12. PortForwarding
```sh
kubectl --namespace rasa port-forward svc/rasa-web 8080:8080
```
now we can access the rasa service.
