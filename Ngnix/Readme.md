## proxy setting 
```
    server {
        listen       443;
        server_name  xxxx;
        ssl on;
        ssl_certificate xxx.crt;
        ssl_certificate_key xxxx.key;

        ssl_session_timeout 5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers RC4:HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers on;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;
        location / {
            proxy_set_header        Host $host;
            proxy_set_header        X-Real-IP $remote_addr;
            proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header        X-Forwarded-Proto $scheme;

            # Fix the “It appears that your reverse proxy set up is broken" error.
            proxy_pass          http://xxxx:8080;
            proxy_read_timeout  90;

            proxy_redirect      http://xxxx:8080 https://xxxx;
        }

```



## yum install ngnix 
https://blog.csdn.net/u012486840/article/details/52610320  
https://www.nginx.com/resources/wiki/start/topics/tutorials/install/  
