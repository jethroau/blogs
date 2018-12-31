## Get
```
kubectl get pods
```

## Deplopyment 
```
kubectl apply -f xxx.yml
```

## Scale
```
kubectl scale --replica=3 deployment/<container-name>  
kubectl autoscale deployment <container-name> --cpu-percent=50 --min=2 --max=10
kubectl get hpa
```
