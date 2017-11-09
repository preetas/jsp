# Instructions

In order to use a Spring Boot project which is packaged as a WAR and which contains by example JSP pages, JSP Standard Tag Library (JSTL), ... some modifications are 
required to build/deploy correctly the project as a container on OpenShift or Kubernetes platform.

The modifications described hereafter assume that you use the `Fabric8 Maven Plugin` like also the `Spring Boot Maven Plugin`

- Enable the repackaging of the WAR to include the Spring Boot classes used to launch the application and to update 
  the MANIFEST.mf file definition with the following properties :
  - `Main-Class: org.springframework.boot.loader.WarLauncher`
  - `Spring-Boot-Classes: WEB-INF/classes/`
  - `Spring-Boot-Lib: WEB-INF/lib/`
  - `Spring-Boot-Version: 1.5.8.RELEASE`
  
  Set this option using the goal repackage of the Spring Boot maven plugin. See the xml tag `<execution><execution><goals><goal>repackage</goal>` defined
  under the `Spring Boot maven plugin`
- Add an env var `<ARTIFACT_COPY_ARGS>*.war</ARTIFACT_COPY_ARGS>` to specify during the creation of the Docker image to copy the war file instead of the 
  uber jar which is the by default option when a Microservice is created. This en var will be used by OpenShift during the S2I Build Process with the war binary content
  pushed.
  
  Such a configuration should be defined using the Fabric8 Maven Configuration part - see `<configuration>`
  xml tag defined for the `<artifactId>fabric8-maven-plugin</artifactId>`
- Define next, an env var `JAVA_APP_JAR=mvc-jsp-0.0.1-SNAPSHOT.war` for the DeploymentConfig file to specify to use the war included within the Docker image 
  as the Java application to be launched.
  
  This modification can be done by specifying this env var as a parameter to pass to the container definition to be created when Openshift will create a pod and ultimately
  a linux container using the definition defined within the `DeploymentConfig` resource. The env var must be specified within the `src/main/fabric8/deployment.yml` file
  that the Fabric8 Maven Plugin will use.

## Deploy project
```bash
mvn clean fabric8:deploy -Popenshift 
```

   

