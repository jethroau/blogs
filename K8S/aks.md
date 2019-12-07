# Quick start
https://docs.microsoft.com/zh-cn/azure/aks/kubernetes-walkthrough-portal?view=azure-cli-latest

# Three minutes guide
1. create AKS cluster via portal. 

2. connected with AKS cluster 
set up connection to AKS cluster
```
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
kubectl get nodes
kubectl get namespaces
```

3. deploy outside container image to AKS cluster
```
kubectl apply -f azure-vote.yaml

--- azure-vote.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: azure-vote-front
...
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  type: LoadBalancer
  ports:
  - port: 80
  selector: 
    app: azure-vote-front   
``` 

4. check created deployment and service 
```
kubectl get service azure-vote-front --watch
```
```
NAME               TYPE           CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.X.X   52.X.X.X     80:30572/TCP   6s
```
52.X.X.X is exposed public IP address to access AKS service and POD  


5. create Azure Container Register and connected the ACR with AKS cluster. 
```
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
az acr login --name <acrName>

docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
docker images
docker push <acrLoginServer>/azure-vote-front:v1

az acr repository list --name <acrName> --output table

az aks update -n myAKSCluster -g myResourceGroup --attach-acr <acrName>
```

6. Deploy your ACR image to AKS cluster
```
--- azure-vote-all-in-one-redis.yaml
containers:
- name: azure-vote-front
  image: <acrName>.azurecr.io/azure-vote-front:v1

----
kubectl apply -f azure-vote-all-in-one-redis.yaml

```

7. Sacling 
```
--- manual scale
kubectl scale --replicas=5 deployment/azure-vote-front
kubectl get pods
                                   READY     STATUS    RESTARTS   AGE
azure-vote-back-2606967446-nmpcf    1/1       Running   0          15m
azure-vote-front-3309479140-2hfh0   1/1       Running   0          3m
azure-vote-front-3309479140-bzt05   1/1       Running   0          3m
azure-vote-front-3309479140-fvcvm   1/1       Running   0          3m
azure-vote-front-3309479140-hrbf2   1/1       Running   0          15m
azure-vote-front-3309479140-qphz8   1/1       Running   0          3m

---- auto scale
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
kubectl get hpa

NAME               REFERENCE                     TARGETS    MINPODS   MAXPODS   REPLICAS   AGE
azure-vote-front   Deployment/azure-vote-front   0% / 50%   3         10        3          2m
```

8. Upgrade k8s version
```
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table

az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.14.6

az aks show --resource-group myResourceGroup --name myAKSCluster --output table
```

## Reference
[Azure AKS Guideline](https://github.com/jethroau/blogs/blob/master/azure/aks.md)   
[Youtube learning video](https://azure.microsoft.com/en-us/resources/kubernetes-learning-path/)  
[Kubectl create/delete deployment](https://blog.csdn.net/liumiaocn/article/details/73913597)  