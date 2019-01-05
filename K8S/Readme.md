## Get
```
kubectl get nodes
kubectl describe node <node-name> 

kubectl get pods [--namespace=xxx]
kubectl describe pod <pod-name>

kubectl get endpoints

kubectl get deployments
kubectl get rs
kubectl describe deployments

kubectl get service
kubectl get service <service-name> -o yaml

kubectl get namespaces

```
>  PodIP + containerPort = endpoint  
>  Pod Volume = all containers volume in a Pod  
>  resource:{request:{cpu:?m, memory:?Mi}, limit:{cpe:?m, memory:?Mi}}, CPU: 100m = 0.1 CPU,  Memory: 128Mi = 128Mib   
>  Label and Label Selector {matchLabeles, matchExpressions}   
>  ReplicaSet and RelicationController, ReplicaSet label support Selector  
>  Deployment replace ReplicaSet and RC  
> NodeIP: physical ip address and can be access by public  
> PodIP: endpoints ip address and can be access within Pod   
> ClusterIP： service ip address and can be access within cluster.   
>

## Deplopyment 
```
kubectl apply -f xxx.yml
kubectl create -f xxx.yaml
kubectl get deployments
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:v2
kubectl set image deployment nginx-deployment nginx=nginx:1.9.1
```


## Scale
```
kubectl scale --replica=3 deployment/<container-name>  
kubectl sacle rc <rc-name> --replica=? 
kubectl autoscale deployment <deployment-name> --cpu-percent=50 --min=2 --max=10
kubectl get hpa
```
> hpa = horizontl pod autoscaling 
>
>

## Official reference. 
[Kubernetes Concepts](https://kubernetes.io/docs/concepts/)  
[Kubernetes Workload/Controller/Deployment](https://kubernetes.io/zh/docs/concepts/workloads/controllers/deployment/)  
