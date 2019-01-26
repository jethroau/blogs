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
