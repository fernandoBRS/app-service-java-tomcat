# Java apps on Azure App Service with Apache Tomcat

Currently the Azure App Service supports Apache Tomcat 8.5 and 9.0 (Public Preview). There are some options to deploy a Java app given this prerequisite in a Linux environment:

- Deployment through Azure CLI + Maven plugin for Azure Web Apps
- Deployment through Docker container

## Deploy with Azure CLI + Maven plugin

Create a new Maven project:

    $ mvn archetype:generate -DgroupId=example.demo -DartifactId=helloworld -DarchetypeArtifactId=maven-archetype-webapp

Edit your `pom.xml` file to add the Tomcat plugin:

```xml
<plugins>
    <plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.4.0</version>
    <configuration>

        <!-- Web App information -->
        <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
        <appName>${WEBAPP_NAME}</appName>
        <region>${REGION}</region>

        <!-- Java Runtime Stack for Web App on Linux-->
        <linuxRuntime>tomcat 8.5-jre8</linuxRuntime>

    </configuration>
    </plugin>
</plugins>
```

*Note: The POM is configured to use Tomcat 8.5. If you want to use Tomcat version higher than 8.5, edit the `<linuxRuntime>` tag.*

Deploy the project with the Maven plugin by adding the resource group, web app name and the web app region:

    $ mvn azure-webapp:deploy \
    -Denv.RESOURCEGROUP_NAME=YOUR_RESOURCE_GROUP \
    -Denv.WEBAPP_NAME=YOUR_WEB_APP_NAME \
    -Denv.REGION=YOUR_APP_REGION

<!-- ## Docker

Edit your `pom.xml` file to add the Tomcat plugin:

```xml
<plugins>
    <plugin>
        <groupId>org.codehaus.cargo</groupId>
        <artifactId>cargo-maven2-plugin</artifactId>
        <version>1.7.0</version>
        <configuration>
            <container>
            <containerId>tomcat8x</containerId>
            <zipUrlInstaller>
                <url>http://repo1.maven.org/maven2/org/apache/tomcat/tomcat/8.5.9/tomcat-8.5.9.zip</url>
            </zipUrlInstaller>
            </container>
        </configuration>
    </plugin>
</plugins>
```

Create the .war project file:

    $ mvn package

Run the application:

    $ mvn cargo:run -->
