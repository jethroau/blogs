## Limits
若要使用自动缩放程序，你的 Pod 中的所有容器和你的 Pod 必须定义了 CPU 请求和限制。
在部署中，前端容器已请求了 0.25 个 CPU，限制为 0.5 个 CPU。 这些资源请求和限制的定义方式如以下示例代码片段所示：  
```
resources:
  requests:
     cpu: 250m
  limits:
     cpu: 500m
```     
