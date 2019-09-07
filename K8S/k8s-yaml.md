## Node
```
kubectl get nodes
kubectl describe node <node_name>
```

## POD
endpoint = Pod IP + container PortNo
```
kubectl create -f pod.yaml
kubectl get pods
kubectl describe pod <pod_name>
kubectl delete pod <pod_name>
```
YAML template:   
```yaml
apiVersion: v1
kind: Pod
metadata: 
    name: xxx-system ##system name
    labels:
         app: xxx-system #system name          
         release: YYYYMMDD # release date time or version 
         environment: production # or uat or dev
    spec: 
        container:
        - name: web-xxx-xxx #git project name
          image: <docker image path>
          ports:
          - containerPort: 80 #443 for TLS or other port
          resource:
            requests:
                memory : "64Mi"  # 64m
                cpu: "250m"  #0.25 CPU
            limits: 
                memory: "128Mi"  # 128m
                cpu: "500m" #0.5 CPU

```

## Replication Set
```
kubectl scale rc redis-slave --replicas=2
```

## Deployment
```
kubectl create -f deployment.yaml
kubectl get deployments
kubectl describe deployment <deploy_name>
```

YAML template:
```yaml
apiVersion: extensions/v1beta1
 kind: Deployment
 metadata:
   name: xxx-system ##system name 
 spec:
   replicas: 2
   template:
     metadata:
       labels:
         app: xxx-system #system name          
         release: YYYYMMDD # release date time or version 
         environment: production # or uat or dev
     spec:
       containers:
         - name: web-xxx-frontend #git project name 
           image: <docker image path>
           ports:
             - containerPort: 8443
         - name: web-xxx-api #git project name
           image: <docker image path>
           ports:
             - containerPort: 8444
```


