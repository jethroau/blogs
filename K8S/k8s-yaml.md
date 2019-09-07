## Node
```
kubectl get nodes
kubectl describe node <node_name>
```

## POD
endpoint = Pod IP + container PortNo
```
kubectl get pods
kubectl describe pod <pod_name>
```
YAML:   
```yaml
apiVersion: v1
kind: Pod
metadata: 
    name: web-xxx-xxx #git project name
    labels: 
        name: web-xxx-xxx # git project name / frontend-xxx-web / middleware-xxx-api / backed-xxx-service
        release: 1.0 # git brach/tag version 
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

##