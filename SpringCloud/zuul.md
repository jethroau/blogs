## infra-zuul-server.yml
```
zuul: 
  ignored-services: '*'
  routes: 
    ms-talent-api: 
      path: /talent/**
      sensitive-headers: Cookie, Set-Cookie
      # stripPrefix=true, http://localhost/talent/1 => http://localhost:9002/1
      # stripPrefix=false, http://localhost/talent/1 => http://localhost:9002/talent/1
      stripPrefix: false 
    ms-config-client: 
      path: /client/**
```

## Ref
[SpringCloud（十一）zuul网关 fallback](https://blog.csdn.net/u012326462/article/details/80671472)  
