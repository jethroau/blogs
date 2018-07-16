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


