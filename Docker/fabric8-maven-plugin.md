## pom.xml
```xml
<build>
<plugins>
<plugin>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-maven-plugin</artifactId>
</plugin>

<plugin>
	<groupId>io.fabric8</groupId>
	<artifactId>docker-maven-plugin</artifactId>
	<version>0.23.0</version>
	<configuration>
		<dockerHost>tcp://192.168.50.104:4243</dockerHost>
		<images>
			<image>
				<name>${project.artifactId}:${project.version}</name>
				<build>
					<from>openjdk:8-jdk-alpine</from>
					<maintainer>jethro</maintainer>
					<assembly>
						<descriptorRef>artifact</descriptorRef>
					</assembly>
					<tags>
						<tag>latest</tag>
						<tag>${project.version}</tag>
					</tags>
					<ports>
						<port>8080</port>
					</ports>
					<volumes>
						<volume>/tmp</volume>
					</volumes>
					<entryPoint>
						<exec>
							<arg>java</arg>
							<arg>-Djava.security.egd=file:/dev/./urandom</arg>
							<arg>-jar</arg>
							<arg>/maven/${project.artifactId}-${project.version}.jar</arg>
						</exec>
					</entryPoint>
				</build>
				<run>
					<ports>
						<port>80:8080</port>
					</ports>
				</run>
			</image>
		</images>
	</configuration>
</plugin>
</plugins>
</build>
```

## build
```
mvn clean package docker:build
```

## check build and run
```
docker image ls
docker run -it -p 8080:80 ja-springboot-hello

```

## reference 
http://dmp.fabric8.io/  
https://blog.csdn.net/alinyua/article/details/81094240  
http://www.cnblogs.com/fairjm/p/docker_maven_springboot.html  
https://blog.csdn.net/wangfei0904306/article/details/72643456  
