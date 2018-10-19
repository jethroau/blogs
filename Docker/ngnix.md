## Simple nginx
```
docker run -d -p 8090:80 --rm -v $PWD/html:/usr/share/nginx/html --privileged=true nginx
```

## Reference
[OFFICIAL REPOSITORY nginx](https://hub.docker.com/_/nginx/)  
[Docker Compose + Spring Boot + Nginx + Mysql 实践](http://ityouknow.com/springboot/2018/03/28/dockercompose-springboot-mysql-nginx.html)  
[基于Nginx、Node.js和Redis的Docker工作流](http://dockone.io/article/291)   
[Authenticate proxy with nginx](https://docs.docker.com/registry/recipes/nginx/#setting-things-up)  
[nginx配置文件nginx.conf超详细讲解](https://www.cnblogs.com/liang-wei/p/5849771.html)  
[nginx超详细讲解之server,log](https://blog.csdn.net/u014459326/article/details/53366921)


## nginx.conf
```
user  nginx;
worker_processes  2;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  2048;
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
    keepalive_timeout  60;
    #gzip  on;
    server_tokens off;
    ssi off;
    autoindex off;

    #include /etc/nginx/conf.d/*.conf;

    upstream node-app {
              server 192.168.50.104:8080;  #ip address
              #server nodejs-app;  #docker compose service id
    }

server {
        listen
