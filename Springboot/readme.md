# Basic
## download template from initializer 
1. open https://start.spring.io/
2. select Maven Project and Java 
3. input group name "io.jethro"
4. input Artifact "ja-springboot-hello"
5. click Options and input name = "hello", description = "Jethro Practice project for Spring boot", packaging = "io.jethro.springboot.hello"
6. click generate

## import into eclipse maven project
1. copy ja-springboot-hello.zip into eclipse work space
2. unzip the package 
3. open eclise and import a project, select existing Maven projects
4. select ja-spring-hello. 
5. ensure your Maven environemnt is ready in eclipse. 
6. if there is any maven error, right click project, select Maven => update project => select "force update..."

## maven run 
1. select pom.xml and right click
2. select Run as, and then select Maven build...
3. input goal "spring-boot:run"
4. see the springboot logo in console
```
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.1.3.RELEASE)
```

# Springboot Web
## pom.xml
```xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
```

## create controller 
```java
package io.jethro.springboot.hello.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {
	
	@GetMapping("/sayHello") 
	public String sayHello() {
		return "Hello World!";	
	}

}
```
## edit src/main/resource/application.properties 
```properties
server.port=80
```

## maven run 
1. right click project "ja-springboot-hello"
2. Run as => Maven build... => input goal "spring-boot:run"  => run

## Test result
1. open chrom and input "http://localhost/sayHello"
2. "Hello world!" is displayed. 





## reference
https://www.cnblogs.com/ityouknow/p/5662753.html  
