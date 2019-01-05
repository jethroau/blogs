## Get
```
kubectl get nodes
kubectl describe node <node-name> 

kubectl get pods
kubectl get service

kubectl get deployments
kubectl describe deployments

kubectl get namespaces

```
>  PodIP + containerPort = endpoint  
>  Pod Volume = all containers volume in a Pod  
>


## Deplopyment 
```
kubectl apply -f xxx.yml
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:v2
kubectl set image deployment nginx-deployment nginx=nginx:1.9.1

```

## Scale
```
kubectl scale --replica=3 deployment/<container-name>  
kubectl autoscale deployment <container-name> --cpu-percent=50 --min=2 --max=10
kubectl get hpa
```
## Official reference. 
[Kubernetes Concepts](https://kubernetes.io/docs/concepts/)  
[Kubernetes Workload/Controller/Deployment](https://kubernetes.io/zh/docs/concepts/workloads/controllers/deployment/)  
