# Instructions

Tricks to use to deploy a Spring Boot WAR project containing by example a JSP page

- Add first an env var `<ARTIFACT_COPY_ARGS>*.war</ARTIFACT_COPY_ARGS>` to specify to copy a war during the creation of the Docker image when 
  S2I build process is running. Such a configuration should be defined within the Fabric8 Maven Configuration part - see `<configuration>`
- Define next, an env var for the DeploymentConfig to define the Java application to be launched `JAVA_APP_JAR=mvc-jsp-0.0.1-SNAPSHOT.war`.
  See the `src/main/fabric8/deployment.yml` file.

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


   

