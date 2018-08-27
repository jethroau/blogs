## command line
```
mvn deploy:deploy-file \
    -Dfile=my-java-project.jar \
    -DgroupId=io.jethro.dev \
    -DartifactId=myProjectID \
    -Dversion=3.12 \
    -Dpackaging=jar \
    -DrepositoryId=myID \
    -Durl=http://localhost:8081/repository/my-hosted/
```
