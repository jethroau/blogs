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
CentOS7 (4GB Memory) (Docker version: 1.13.1)
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
Docker 18.06.0-ce (1GB)
```
docker run -itd -m 50m --rm -v /home/spp/spp-test-jvm:/app adoptopenjdk/openjdk8-openj9
docker ps
docker exec -it <container-id> /bin/bash
 
java -version
openjdk version "1.8.0_181"
OpenJDK Runtime Environment (build 1.8.0_181-b13)
Eclipse OpenJ9 VM (build openj9-0.9.0, JRE 1.8.0 Linux amd64-64-Bit Compressed References 20180813_291 (JIT enabled, AOT enabled)
OpenJ9   - 24e53631
OMR      - fad6bf6e
JCL      - a05586ac based on jdk8u181-b13)
###############################
root@ef57ee414766:/app# java MemEat
initial free memory:6MB
current memory allocated:8MB
total memory can allocate:25MB
free memory: 6MB
free memory: 7MB
JVMDUMP039I Processing dump event "systhrow", detail "java/lang/OutOfMemoryError" at 2018/08/25 05:28:55 - please wait.
JVMDUMP032I JVM requested System dump using '/app/core.20180825.052855.113.0001.dmp' in response to an event
JVMDUMP010I System dump written to /app/core.20180825.052855.113.0001.dmp
JVMDUMP032I JVM requested Heap dump using '/app/heapdump.20180825.052855.113.0002.phd' in response to an event
JVMDUMP010I Heap dump written to /app/heapdump.20180825.052855.113.0002.phd
JVMDUMP032I JVM requested Java dump using '/app/javacore.20180825.052855.113.0003.txt' in response to an event
JVMDUMP010I Java dump written to /app/javacore.20180825.052855.113.0003.txt
JVMDUMP032I JVM requested Snap dump using '/app/Snap.20180825.052855.113.0004.trc' in response to an event
JVMDUMP010I Snap dump written to /app/Snap.20180825.052855.113.0004.trc
JVMDUMP013I Processed dump event "systhrow", detail "java/lang/OutOfMemoryError".
Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
        at MemEat.main(MemEat.java:10)
##########################################
root@ef57ee414766:/app# java -XX:+UseContainerSupport  MemEat
initial free memory:6MB
current memory allocated:8MB
total memory can allocate:25MB
free memory: 6MB
free memory: 7MB
JVMDUMP039I Processing dump event "systhrow", detail "java/lang/OutOfMemoryError" at 2018/08/25 05:30:11 - please wait.
JVMDUMP032I JVM requested System dump using '/app/core.20180825.053011.135.0001.dmp' in response to an event
JVMDUMP010I System dump written to /app/core.20180825.053011.135.0001.dmp
JVMDUMP032I JVM requested Heap dump using '/app/heapdump.20180825.053011.135.0002.phd' in response to an event
JVMDUMP010I Heap dump written to /app/heapdump.20180825.053011.135.0002.phd
JVMDUMP032I JVM requested Java dump using '/app/javacore.20180825.053011.135.0003.txt' in response to an event
JVMDUMP010I Java dump written to /app/javacore.20180825.053011.135.0003.txt
JVMDUMP032I JVM requested Snap dump using '/app/Snap.20180825.053011.135.0004.trc' in response to an event
JVMDUMP010I Snap dump written to /app/Snap.20180825.053011.135.0004.trc
JVMDUMP013I Processed dump event "systhrow", detail "java/lang/OutOfMemoryError".
Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
        at MemEat.main(MemEat.java:10)


```

## Experiment4
Docker 18.06.0-ce (1GB)
```
docker run -itd -m 50m --rm -v /home/spp/spp-test-jvm:/app java:openjdk-8u111


root@3fdb53ae7125:/app# java MemEat
initial free memory:15MB
current memory allocated:15MB
total memory can allocate:239MB
free memory: 7MB
free memory: 11MB
free memory: 14MB
free memory: 6MB
free memory: 37MB
free memory: 29MB
free memory: 21MB
free memory: 13MB
free memory: 82MB
free memory: 74MB
free memory: 66MB
Killed
###################
root@3fdb53ae7125:/app# java -XX:+UnlockExperimentalVMOptions -XX:+UseCGroupMemoryLimitForHeap MemEat
Unrecognized VM option 'UseCGroupMemoryLimitForHeap'
Error: Could not create the Java Virtual Machine.
Error: A fatal exception has occurred. Program will exit.

```

## Experiment5
```
docker run -itd -m 50m --rm -v /home/spp/spp-test-jvm:/app adoptopenjdk/openjdk8:latest

root@a5b70fbee042:/app# java -version
Picked up JAVA_TOOL_OPTIONS: -XX:+UnlockExperimentalVMOptions -XX:+UseCGroupMemoryLimitForHeap
openjdk version "1.8.0-adoptopenjdk"
OpenJDK Runtime Environment (build 1.8.0-adoptopenjdk-jenkins_2018_05_19_01_00-b00)
OpenJDK 64-Bit Server VM (build 25.71-b00, mixed mode)

############################################
root@a5b70fbee042:/app# java MemEat
Picked up JAVA_TOOL_OPTIONS: -XX:+UnlockExperimentalVMOptions -XX:+UseCGroupMemoryLimitForHeap
initial free memory:7MB
current memory allocated:7MB
total memory can allocate:25MB
free memory: 7MB
free memory: 7MB
Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
        at MemEat.main(MemEat.java:10)

```

## Experiment6
```
docker run -itd -m 50m --rm -v /home/spp/spp-test-jvm:/app openjdk:8-jdk-alpine
docker-enter 8ba5912b605e

8ba5912b605e:~# java -version
openjdk version "1.8.0_171"
OpenJDK Runtime Environment (IcedTea 3.8.0) (Alpine 8.171.11-r0)
OpenJDK 64-Bit Server VM (build 25.171-b11, mixed mode)

####################
8ba5912b605e:/app# java MemEat
initial free memory:15MB
current memory allocated:15MB
total memory can allocate:239MB
free memory: 13MB
...
free memory: 7MB
free memory: 90MB
free memory: 88MB
...
Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
        at MemEat.main(MemEat.java:10)

##############################################
8ba5912b605e:/app# java -XX:+UnlockExperimentalVMOptions -XX:+UseCGroupMemoryLimitForHeap MemEat
initial free memory:7MB
current memory allocated:7MB
total memory can allocate:25MB
free memory: 5MB
free memory: 3MB
free memory: 1MB
free memory: 6MB
free memory: 4MB
free memory: 2MB
free memory: 0MB
free memory: 8MB
free memory: 6MB
free memory: 4MB
free memory: 2MB
Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
        at MemEat.main(MemEat.java:10)
```

## Experiment7
```
docker run -itd -m 50m --rm -v /home/spp/spp-test-jvm:/app adoptopenjdk/openjdk8-openj9:slim

[root@docker-ce spp-test-jvm]# docker exec -it 332e43260424 /bin/bash
root@332e43260424:/# java -version
openjdk version "1.8.0_181"
OpenJDK Runtime Environment (build 1.8.0_181-b13)
Eclipse OpenJ9 VM (build openj9-0.9.0, JRE 1.8.0 Linux amd64-64-Bit Compressed References 20180813_291 (JIT enabled, AOT enabled)
OpenJ9   - 24e53631
OMR      - fad6bf6e
JCL      - a05586ac based on jdk8u181-b13)
####################
root@332e43260424:/app# java MemEat
initial free memory:6MB
current memory allocated:8MB
total memory can allocate:25MB
free memory: 4MB
free memory: 2MB
free memory: 3MB
free memory: 4MB
free memory: 2MB
free memory: 6MB
free memory: 4MB
free memory: 8MB
free memory: 6MB
free memory: 4MB
free memory: 2MB
JVMDUMP039I Processing dump event "systhrow", detail "java/lang/OutOfMemoryError" at 2018/08/25 06:07:37 - please wait.
JVMDUMP032I JVM requested System dump using '/app/core.20180825.060737.39.0001.dmp' in response to an event
JVMDUMP010I System dump written to /app/core.20180825.060737.39.0001.dmp
JVMDUMP032I JVM requested Heap dump using '/app/heapdump.20180825.060737.39.0002.phd' in response to an event
JVMDUMP010I Heap dump written to /app/heapdump.20180825.060737.39.0002.phd
JVMDUMP032I JVM requested Java dump using '/app/javacore.20180825.060737.39.0003.txt' in response to an event
JVMDUMP010I Java dump written to /app/javacore.20180825.060737.39.0003.txt
JVMDUMP032I JVM requested Snap dump using '/app/Snap.20180825.060737.39.0004.trc' in response to an event
JVMDUMP010I Snap dump written to /app/Snap.20180825.060737.39.0004.trc
JVMDUMP013I Processed dump event "systhrow", detail "java/lang/OutOfMemoryError".
Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
        at MemEat.main(MemEat.java:10)

```



## Reference
[Getting Memory Usage in Linux and Docker](https://shuheikagawa.com/blog/2017/05/27/memory-usage/)  
[Java和Docker限制的那些事儿](http://www.techug.com/post/java-and-docker-memory-limits.html)  
[adoptopenjdk/openjdk8-openj9](https://hub.docker.com/r/adoptopenjdk/openjdk8-openj9/)  
[openj9](https://www.eclipse.org/openj9/docs/xxusecontainersupport/)  
[OpenJ9 versus HotSpot](http://royvanrijn.com/blog/2018/05/openj9-jvm-shootout/)
