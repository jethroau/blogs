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
2. select Maven build...
3. input goal "spring-boot:run"
