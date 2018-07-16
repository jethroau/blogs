## pom.xml
```maven
<properties>
	<docker.image.prefix>springboot</docker.image.prefix>
</properties>

<build>
	<plugins>
		<plugin>
			<groupId>com.spotify</groupId>
			<artifactId>docker-maven-plugin</artifactId>
			<version>1.0.0</version>
			<configuration>
				<imageName>${docker.image.prefix}/${project.artifactId}</imageName>
				<dockerDirectory>${project.basedir}/src/main/docker</dockerDirectory>
				<resources>
					<resource>
						<targetPath>/</targetPath>
						<directory>${project.build.directory}</directory>
						<include>${project.build.finalName}.jar</include>
					</resource>
				</resources>
			</configuration>
		</plugin>
	<plugins>	
</build>
```

## src/main/docker/Dockerfile
```docker
FROM openjdk:8-jdk-alpine
VOLUME /tmp
ADD spp_springboot_hello-1.0.jar app.jar
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
```

## $MAVEN_HOME/conf/setting.xml
```xml
<pluginGroups>  
    <pluginGroup>com.spotify</pluginGroup>  
</pluginGroups>
```

## build docker image
````
mvn clean package docker:build
````

## run docker image
```
docker run -p 80:8080 -t springboot/spp_springboot_hello
```
> **-p** public port : docker internal port, publish docker inernal service to outside prot
> **-t** Allocate a pseudo-TTY, open terminal when runs docker image

```
 docker run -p 80:8080 -d --name springboot-hello springboot/spp_springboot_hello 
```
> **-d** run as a backgroup job

```
 docker ps
 docker stop springboot-hello
 ```

## reference
http://www.ityouknow.com/springboot/2018/03/19/spring-boot-docker.html
http://www.cnblogs.com/rongfengliang/p/6669152.html


