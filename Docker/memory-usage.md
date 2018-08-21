By default, Docker containers have no resource constraints  


## check CPU, memory and IO usage
```docker
docker stats
docker stats <container id>
```

## dockerfile
```
FROM openjdk:8-jdk-alpine
VOLUME /tmp
ADD spp_springboot_hello-1.0.jar app.jar
ENV JAVA_OPTS -XX:+UnlockExperimentalVMOptions -XX:+UseCGroupMemoryLimitForHeap
ENTRYPOINT ["java","${JAVA_OPTS}", "-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
```

## Reference
[Getting Memory Usage in Linux and Docker](https://shuheikagawa.com/blog/2017/05/27/memory-usage/)  
[Java和Docker限制的那些事儿](http://www.techug.com/post/java-and-docker-memory-limits.html)  
[adoptopenjdk/openjdk8-openj9](https://hub.docker.com/r/adoptopenjdk/openjdk8-openj9/)  
[openj9](https://www.eclipse.org/openj9/docs/xxusecontainersupport/)  
