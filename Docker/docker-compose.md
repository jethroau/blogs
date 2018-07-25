
## create a folder 
```
mkdir spp_phyon
```

## vi app.py
```
from flask import Flask
from redis import Redis

app = Flask(__name__)
redis = Redis(host='redis', port=6379)

@app.route('/')
def hello():
  count = redis.incr('hits')
  return 'Hello World! 该页面已被访问 {} 次。\n'.format(count)
if __name__ == "__main__":
  app.run(host="0.0.0.0", debug=True)
```

## Dockerfile
```
FROM python:3.6-alpine
ADD . /code
WORKDIR /code
RUN pip install redis flask
CMD ["python", "app.py"]
```

## docker-compose.yml
```yml
version: '3'
services:
  web:
    build: .
    ports:
      - "9000:5000"

  redis:
    image: "redis:alpine"
```

##  Create and start containers
```
docker-compose up
```
## list container
```bash
[root@docker spp_python]#docker-compose ps
       Name                     Command               State    Ports
--------------------------------------------------------------------
spp_python_redis_1   docker-entrypoint.sh redis ...   Exit 0
spp_python_web_1     python app.py                    Exit 0

[root@docker spp_python]# docker-compose start
Starting web   ... done
Starting redis ... done

[root@docker spp_python]# docker-compose stop
Stopping spp_python_web_1   ... done
Stopping spp_python_redis_1 ... done

```

