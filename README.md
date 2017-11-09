# Instructions

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


   

