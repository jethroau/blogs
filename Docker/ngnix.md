## Simple nginx
```
docker run -d -p 8090:80 --rm -v $PWD/html:/usr/share/nginx/html --privileged=true nginx
```

## Reference
[Docker Compose + Spring Boot + Nginx + Mysql 实践](http://ityouknow.com/springboot/2018/03/28/dockercompose-springboot-mysql-nginx.html)  
[基于Nginx、Node.js和Redis的Docker工作流](http://dockone.io/article/291)  


```
docker run \
-p 8080:80 \
--name nginx-1 \
-v `pwd`/log/:/var/log/nginx \
-v `pwd`/nginx.conf:/etc/nginx/nginx.conf \
-v `pwd`/html/:/usr/share/nginx/html \
-d \
nginx 
```


/etc/nginx/nginx.conf  
```
root@8185bbe9489d:/etc/nginx# cat nginx.conf

user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}
root@8185bbe9489d:/etc/nginx#

```
