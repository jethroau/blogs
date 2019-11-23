## Memory limit for a Namespace
https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/memory-default-namespace/  
```
kubectl create namespace uat-jethro-io
kubectl apply -f limit-memory-range.yaml --namespace uat-jethro-io

---

apiVersion: v1
kind: LimitRange
metadata:
  name: limit-memory-range
spec: 
  limits:
  - default: 
      memory: 512Mi
    defaultRequest:
      memory: 256Mi
    type: Container
    
---

kubectl create -f https://k8s.io/examples/admin/resource/memory-defaults-pod.yaml --namespace=uat-jethro-io
kubectl get pod default-mem-demo --output=yaml --namespace=uat-jethro-io
    
```





## Limits
若要使用自动缩放程序，你的 Pod 中的所有容器和你的 Pod 必须定义了 CPU 请求和限制。
在部署中，前端容器已请求了 0.25 个 CPU，限制为 0.5 个 CPU。 这些资源请求和限制的定义方式如以下示例代码片段所示：  
```
spec: 
  containers:
  - image: xxx
    name: xxx
    resources:
      requests:
         cpu: 250m
      limits:
         cpu: 500m
```     
