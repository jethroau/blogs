## deployment
```
$ kubectl apply -f azure-vote-all-in-one-redis.yaml

deployment "azure-vote-front" created
service "azure-vote-front" created

--- azure-vote-all-in-one-redis.yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: azure-vote-front
spec:
  replicas: 1
  strategy:
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
  minReadySeconds: 5
  template:
    metadata:
      labels:
        app: azure-vote-front
    spec:
      nodeSelector:
        "beta.kubernetes.io/os": linux
      containers:
      - name: azure-vote-front
        image: <ACR-Domain>/azure-vote-front:v1
        ports:
        - containerPort: 80
        resources:
          requests:
            cpu: 250m
          limits:
            cpu: 500m
        env:
        - name: REDIS
          value: "azure-vote-back"
---
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

## integrate ACR with AKS
```
az aks update -n myAKSCluster -g myResourceGroup --attach-acr <ACR-Domain>
```

## scale
```
kubectl scale --replicas=5 deployment/azure-vote-front
kubectl autoscale deployment azure-vote-front --cpu-percent=50 --min=3 --max=10
kubectl get hpa
```

## update image version
```
kubectl set image deployment azure-vote-front azure-vote-front=<ACR-Domain>/azure-vote-front:v2
```
