## Simple nginx
```
docker run -d -p 8090:80 --rm -v $PWD/html:/usr/share/nginx/html --privileged=true nginx
```

## Reference
[Docker Compose + Spring Boot + Nginx + Mysql 实践](http://ityouknow.com/springboot/2018/03/28/dockercompose-springboot-mysql-nginx.html)  
[基于Nginx、Node.js和Redis的Docker工作流](http://dockone.io/article/291)  

## nginx.conf
```
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

    #include /etc/nginx/conf.d/*.conf;
    server {
        listen 80;
        charset utf-8;
        access_log off;

        location / {
            proxy_pass http://app:8080;
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
    }

}
```


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

## Dockerfile
nginx
```
FROM nginx

# Copy custom configuration file from the current directory
COPY nginx.conf /etc/nginx/nginx.conf
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

/etc/nginx/nginx.conf.default 
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
