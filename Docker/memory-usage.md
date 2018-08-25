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

## Experiment1
CentOS7 (4GB Memory) (Docker version: 1.13.1)
```
docker run -m 50m --rm -itd -v /home/spp/spp-test-jvm:/app openjdk:8 /bin/bash
docker-enter <container-id>

java -version
openjdk version "1.8.0_181"
########################################################
java -XX:+UnlockExperimentalVMOptions -XX:+UseCGroupMemoryLimitForHeap MemEat
initial free memory:7MB
current memory allocated:7MB
total memory can allocate:25MB
free memory: 7MB
free memory: 7MB
Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
        at MemEat.main(MemEat.java:10)
########################################################
java MemEat
initial free memory:57MB
current memory allocated:58MB
total memory can allocate:916MB
...
free memory: 33MB
free memory: 25MB
free memory: 447MB
free memory: 439MB
free memory: 431MB
free memory: 423MB
...
Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
        at MemEat.main(MemEat.java:10)
###########################################################

```

## Experiment2
```
docker run -itd -m 50m --rm -v /home/spp/spp-test-jvm:/app java:openjdk-8u111

java -version
openjdk version "1.8.0_111"
OpenJDK Runtime Environment (build 1.8.0_111-8u111-b14-2~bpo8+1-b14)
OpenJDK 64-Bit Server VM (build 25.111-b14, mixed mode)
########################
root@2bc0eeedb0f9:~# java -XX:+UnlockExperimentalVMOptions -XX:+UseCGroupMemoryLimitForHeap MemEat
Unrecognized VM option 'UseCGroupMemoryLimitForHeap'
Error: Could not create the Java Virtual Machine.
Error: A fatal exception has occurred. Program will exit.
#######################
root@2bc0eeedb0f9:/app# java MemEat
initial free memory:57MB
current memory allocated:58MB
...
free memory: 41MB
free memory: 33MB
free memory: 25MB
free memory: 447MB
free memory: 439MB
...
Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
        at MemEat.main(MemEat.java:10)

```

## Experiment3
```
```

## Reference
[Getting Memory Usage in Linux and Docker](https://shuheikagawa.com/blog/2017/05/27/memory-usage/)  
[Java和Docker限制的那些事儿](http://www.techug.com/post/java-and-docker-memory-limits.html)  
[adoptopenjdk/openjdk8-openj9](https://hub.docker.com/r/adoptopenjdk/openjdk8-openj9/)  
[openj9](https://www.eclipse.org/openj9/docs/xxusecontainersupport/)  
[OpenJ9 versus HotSpot](http://royvanrijn.com/blog/2018/05/openj9-jvm-shootout/)
