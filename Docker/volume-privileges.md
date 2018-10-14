## check user in docker container
```
[root@docker]# docker run -ti --rm --entrypoint="/bin/bash" nginx -c "whoami && id"
root
uid=0(root) gid=0(root) groups=0(root)

[root@docker]# docker run -ti --rm --entrypoint="/bin/bash" jenkins -c "whoami && id"
jenkins
uid=1000(jenkins) gid=1000(jenkins) groups=1000(jenkins)
```


## Reference
[谈谈 Docker Volume 之权限管理（一）](https://yq.aliyun.com/articles/53990)  
[Docker Volume 之权限管理](https://www.cnblogs.com/jackluo/p/5783116.html)
