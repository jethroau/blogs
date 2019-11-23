## Memory limit for a Namespace (单个容器而不是所有容器进行限制，就请使用 LimitRange)
https://kubernetes.io/docs/tasks/administer-cluster/manage-resources/memory-default-namespace/  
```
kubectl create namespace uat-jethro-io
kubectl apply -f limit-memory-range.yaml --namespace uat-jethro-io
kubectl get limit-range --namespace=uat-jethro-io --output=yaml

--- limit-memory-range.yaml

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
    
--- check memory quota from a new container in the namespace 

kubectl create -f https://k8s.io/examples/admin/resource/memory-defaults-pod.yaml --namespace=uat-jethro-io
kubectl get pod default-mem-demo --output=yaml --namespace=uat-jethro-io
    

```

## CPE and Memory limit for a namespace  (所有容器进行限制)
```
kubectl create namespace uat-jethro-io
kubectl apply -f limit-cpu-memory.yaml
kubectl get resourcequota --namespace uat-jethro-io --output=yaml

--- limit-cpu-memory.yaml

apiVersion: v1
kind: ResourceQuota
metadata:
  name: limit-cpu-memory
spec:
  hard: 
    requests.cpu: "1"
    requests.memory: 1Gi
    limits.cpu: "2"
    limits.memory: 2Gi
   
-- check Quota




```
- 每个容器必须有内存请求和限制，以及 CPU 请求和限制。
- 所有容器的内存请求总和不能超过1 GiB。
- 所有容器的内存限制总和不能超过2 GiB。
- 所有容器的 CPU 请求总和不能超过1 cpu。
- 所有容器的 CPU 限制总和不能超过2 cpu。



## Limits
若要使用自动缩放程序，你的 Pod 中的所有容器和你的 Pod 必须定义了 CPU 请求和限制。
在部署中，前端容器已请求了 0.25 个 CPU，内存256Mb，限制为 0.5 个 CPU，内存512Mb。 这些资源请求和限制的定义方式如以下示例代码片段所示：  
```
spec: 
  containers:
  - image: xxx
    name: xxx
    resources:
      requests:
         cpu: 250m
         memory: 256Mi
      limits:
         cpu: 500m
         memory: 512Mi
```     
