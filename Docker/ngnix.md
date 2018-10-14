## Simple nginx
```
docker run -d -p 8090:80 --rm -v $PWD/html:/usr/share/nginx/html nginx
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
