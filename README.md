# Instructions

Tricks to be able to deploy a Spring Boot WAR project containing by example a JSP page

- Add and env var `<ARTIFACT_COPY_ARGS>*.war</ARTIFACT_COPY_ARGS>` to the definition of the docker image to be build during S2I Buildconfig process. See `<configuration>`
  tag of the Fabric8 Maven Plugin definition
- Define an env var for the DeploymentConfig `JAVA_APP_JAR=mvc-jsp-0.0.1-SNAPSHOT.war`. See `src/main/fabric8/deployment.yml` file  


## Deploy project
```bash
mvn clean fabric8:deploy -Popenshift 
```

## Manual

- Create BuildConfig
```
oc login ....
oc new-project demo-jsp
oc new-build --binary=true \
   --docker-image registry.access.redhat.com/redhat-openjdk-18/openjdk18-openshift \
   --strategy source \
   --name spring-boot-jsp-war-s2i \
   -e ARTIFACT_COPY_ARGS=*.war
```   
   
## Build project
```
oc start-build --from-file=target/mvc-jsp-0.0.1-SNAPSHOT.war spring-boot-jsp-war-s2i 
```


   

