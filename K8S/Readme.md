## Get
```
kubectl get pods
kubectl get service
```

## Deplopyment 
```
kubectl apply -f xxx.yml
kubectl set image deployment azure-vote-front azure-vote-front=<acrLoginServer>/azure-vote-front:v2
```

## Scale
```
kubectl scale --replica=3 deployment/<container-name>  
kubectl autoscale deployment <container-name> --cpu-percent=50 --min=2 --max=10
kubectl get hpa
```
## Official reference. 
[Workload/Controller/Deployment](https://kubernetes.io/zh/docs/concepts/workloads/controllers/deployment/)  
