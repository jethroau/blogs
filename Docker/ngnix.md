## Simple nginx
```
docker run -d -p 8090:80 --rm -v $PWD/html:/usr/share/nginx/html --privileged=true nginx
```

## Reference
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
        listen 80;
        charset utf-8;
        access_log off;

        add_header X-Frame-Options "SAMEORIGIN";

        #To add basic authentication to v2 use auth_basic setting.
        auth_basic "Registry realm";
        auth_basic_user_file /etc/nginx/conf.d/nginx.htpasswd;

        location / {
                proxy_pass http://node-app; # load balance, refer to upstream
                #proxy_pass http://192.168.x.x:8080; # normal directly proxy
                proxy_set_header Host $host:$server_port;
                proxy_set_header X-Forwarded-Host $server_name;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }

        location /static {
                access_log   off;
                expires      30d;
                alias /app/static;
        }

        location ~* \.(bak|save|sh|sql|mdb|svn|git|old)$ {
                rewrite ^/(.*)$  $host  permanent;
        }
        
        ##security filter XSS, SQL inject....
        if ($request_method !~ ^(GET|HEAD|POST)$ ) {
                return 444;
        }
        if ($query_string ~* "union.*select.*\(") {
                rewrite ^/(.*)$  $host  permanent;
        }
        if ($query_string ~* "concat.*\(") {
                rewrite ^/(.*)$  $host  permanent;
        }

   }
}
```

## node.js 
server.js  
```
'use strict';

const express = require('express');

// Constants
const PORT = 8080;
const HOST = '0.0.0.0';

// App
const app = express();
app.get('/hi', (req, res) => {
   res.send('Hello world!\n');
});

app.listen(PORT, HOST);
console.log(`Running on http://${HOST}:${PORT}`);

```
package.json
```
{
  "name": "docker_web_app",
  "version": "1.0.0",
  "description": "Node.js on Docker",
  "author": "First Last <first.last@example.com>",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.16.1"
  }
}
```

## create basic auth htpasswd
```
docker run --rm --entrypoint htpasswd registry:2 -Bbn jethro jethro >> nginx.htpasswd
```


## Dockerfile
nginx  
```
FROM nginx:alpine

# Copy custom configuration file from the current directory
COPY nginx.conf /etc/nginx/nginx.conf
COPY nginx.htpasswd /etc/nginx/conf.d/nginx.htpasswd
```

node  
```
FROM node:8

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm install
# If you are building your code for production
# RUN npm install --only=production

# Bundle app source
COPY . .

EXPOSE 8080
CMD [ "npm", "start" ]
```

## docker build
```
docker build -t nginx-2 .
docker build -t nodejs-1 .
```


## docker-compose.xml
don't disclose express port, nginx connects with express using docker internal channel.    
```
version: '3'
services:
  nginx:
    image: nginx-2
    ports:
    - 8090:80

  app:
    image: nodejs-1
```

disclose express port for nginx to connect with.  
```
version: '3'
services:
  nginx:
    image: nginx-3
    ports:
    - 8090:80

  app:
    image: nodejs-1
    ports:
    - 8080:8080

```

## run
```
docker-compose up -d
```
