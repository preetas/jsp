# Instructions

Tricks to use to deploy a Spring Boot WAR project containing by example a JSP page

- Enable the repackaging of the WAR to include a MANIFEST.mf file which defines the main class to be used to bootstrap the application `Main-Class: org.springframework.boot.loader.WarLauncher`
  like the `Spring-Boot-Classes, Spring-Boot-Lib, Spring-Boot-Version` parameters.
  Set this option using the goal repackage of the Spring Boot maven plugin. See - `<execution><execution><goals><goal>repackage</goal>` within the pom.xml file
- Add an env var `<ARTIFACT_COPY_ARGS>*.war</ARTIFACT_COPY_ARGS>` to specify to copy the war during the creation of the Docker image when 
  S2I build process is running. Such a configuration should be defined within the Fabric8 Maven Configuration part - see `<configuration>`
  xml tag defined for the `<artifactId>fabric8-maven-plugin</artifactId>`
- Define next, an env var for the DeploymentConfig to specify to use that war as the Java application to be launched `JAVA_APP_JAR=mvc-jsp-0.0.1-SNAPSHOT.war`.
  See the `src/main/fabric8/deployment.yml` file.

## Deploy project
```bash
mvn clean fabric8:deploy -Popenshift 
```

   

