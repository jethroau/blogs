# Noraml frontend and backend POD

###  frontend-localredis-pod.yaml
```
apiVersion: v1
ntend-localredis-pod.yaml
kind: Pod
metadata:
  name: redis-php
  labels:
    name: redis-php
spec:
  containers:
  - name: frontend
    image: kubeguide/guestbook-php-frontend:localredis
    ports:
    - containerPort: 80
  - name: redis
    image: kubeguide/redis-master
    ports:
    - containerPort: 6379

```

### create pod and describe pod
```
kubectl create -f front-localredis-pod.yaml
kubectl get pods
kubectl describe pod redis-php
```


# Share volumen in POD

### pod-volume-applogs.yaml
create volumes `app-logs` and mounts to `/usr/local/tomcat/logs` in **tomcat container** and `/logs` in **logreader**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-pod
spec:
  containers:
  - name: tomcat
    image: tomcat
    ports:
    - containerPort: 8080
    volumeMounts:
    - name: app-logs
      mountPath: /usr/local/tomcat/logs
  - name: logreader
    image: busybox
    command: ["sh", "-c", "tail -f /logs/catalina*.log"]
    volumeMounts:
    - name: app-logs
      mountPath: /logs
  volumes:
  - name: app-logs
    emptyDir: {}
```
    
### logs and enter container
```
kubectl logs volumne-pod -c busybox
kubectl exec -ti volume-pod -c tomcat -- ls /usr/local/tomcat/logs
kubectl exec -ti volume-pod -c tomcat -- tail /usr/local/tomcat/logs
```

