#### Maven :

Java maven example to understand the implementation of log4j for logging example and customizing the logging, adding maven-shaded-plugin to create uber jar (fat jar with all the dependencies). Match the logging pattern to match the pretrained splunk format to send logs to splunk.

" we might just need to define a log4j.properties file and maybe try to match the log4j pretrained format that Splunk knows about "

#### Maven IntelliJ Example:
File >> New >> Project >> Maven >> Next >> (fill below details) >> Finish
GroupId: com.LoggingExample   
ArtificatId: com.LoggingExample   

#### Project Structure:
```xml
- src
  - main
    - java
      - com.logger
        - Logging
    - resources
      - log4j.properties
- LoggingExample.iml
- pom.xml  
```

#### Maven Command
```shell
mvn clean
mvn clean install
mvn package
```

#### Notes:
* If we need to configure logging through log4j.properties then we must have it in the class path ( i.e src/main/resources)
* The name of the folder should be `resources`, if we spelled wrong maven will not refer this folder by default and jar will miss the property file. Also logging configuration will not be applied to the program.

#### Implementation:

1. Logging.java
```java
package com.logger;

import org.apache.log4j.Logger;

public class Logging {
    final static Logger logger = Logger.getLogger(Logging.class);

    public static void main(String[] args) {
        Logging obj = new Logging();
        obj.runMe("Testing");
    }

    private void runMe(String parameter) {
        if (logger.isDebugEnabled()) {
            logger.debug("This is debug : " + parameter);
        }

        if (logger.isInfoEnabled()) {
            logger.info("This is info : " + parameter);
        }
        logger.warn("This is warn : " + parameter);
        logger.error("This is error : " + parameter);
        logger.fatal("This is fatal : " + parameter);
    }
}
```

2. pom.xml
```java
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>LoggingExample</groupId>
    <artifactId>LoggingExample</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <properties>
        <jdk.version>1.8</jdk.version>
        <log4j.version>1.2.17</log4j.version>
    </properties>

    <!-- log4j dependency -->
    <dependencies>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>${log4j.version}</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.6.0</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.6.0</version>
        </dependency>
    </dependencies>

    <build>
        <!-- defining the final build jar name -->
        <finalName>exampleLogger</finalName>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-eclipse-plugin</artifactId>
                <version>2.9</version>
                <configuration>
                    <downloadSources>true</downloadSources>
                    <downloadJavadocs>false</downloadJavadocs>
                    <wtpversion>2.0</wtpversion>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>2.3.2</version>
                <configuration>
                    <source>${jdk.version}</source>
                    <target>${jdk.version}</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

3. Execution
If we try to execute the program once we build the maven project then it will fail for below reasons,

3.1 Not Executable Jar
```java
mvn clean install #this will build and install the maven project
jar -jar target/exampleLogger.jar
```
If we try execute the above command it will fail complaining `no main manifest attribute, in target/exampleLogger.jar`. `-jar` option will only work if the jar file is an executable jar, i.e jar file must have an manifest file with a Main-Class attribute init. We can convert this project to have executable jar via `CLI` or `MavenPlugin` or using `Ant`.

3.2 CLI for Executing the jar
```java
java -cp target/exampleLogger.jar com.logger.Logging
#java -cp project_name_jar project_name_with_main
```
This will also fail complaining `Exception in thread "main" java.lang.NoClassDefFoundError: org/apache/log4j/Logger` as the jar is not packaged with all the dependencies. Or we could have used `maven-jar-plugin` and defined the path to main class in `<mainClass>com.logger.Logging</mainClass>` but during execution it would have complained for the same.

4. Maven-Shaded-Plugin  
The only addition to above project is to implement shaded plugin in pom.xml. So the pom.xml build will look as below.
```xml
<build>
    <finalName>exampleLogger</finalName>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-eclipse-plugin</artifactId>
            <version>2.9</version>
            <configuration>
                <downloadSources>true</downloadSources>
                <downloadJavadocs>false</downloadJavadocs>
                <wtpversion>2.0</wtpversion>
            </configuration>
        </plugin>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>2.3.2</version>
            <configuration>
                <source>${jdk.version}</source>
                <target>${jdk.version}</target>
            </configuration>
        </plugin>

        <!-- maven shaded plugin -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>2.3</version>
            <executions>
                <!-- Run shade goal on package phase -->
                <execution>
                    <phase>package</phase>
                    <goals>
                        <goal>shade</goal>
                    </goals>
                    <configuration>
                        <transformers>
                            <!-- add Main-Class to manifest file -->
                            <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                <mainClass>com.logger.Logging</mainClass>
                            </transformer>
                        </transformers>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

5. log4j.properties
```java
# Root logger option
log4j.rootLogger=DEBUG, CONSOLE

#to enable all
#log4j.rootLogger=DEBUG, CONSOLE, stdout, file

# # Redirect log messages to console
# log4j.appender.stdout=org.apache.log4j.ConsoleAppender
# log4j.appender.stdout.Target=System.out
# log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
# log4j.appender.stdout.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L ++ %m%n
#
# # Redirect log messages to a log file
# log4j.appender.file=org.apache.log4j.RollingFileAppender
# log4j.appender.file.File=C:\\log4j-application.log
# log4j.appender.file.MaxFileSize=5MB
# log4j.appender.file.MaxBackupIndex=10
# log4j.appender.file.layout=org.apache.log4j.PatternLayout
# log4j.appender.file.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %-5p %c{1}:%L :: %m%n

#Putting in console
log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
log4j.appender.CONSOLE.Target=System.err
log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout
log4j.appender.CONSOLE.layout.conversionPattern=%d{yyyy-MM-dd HH:mm:ss,SSS} %-5p -- %m%n
```

6. Build, Package, Run
```shell
mvn clean install
mvn package
java -jar target/exampleLogger.jar
```
The console log should be appeared as we mentioned in the `conversionPattern`. So it will look like
```shell
2018-01-21 23:04:30,614 DEBUG -- This is debug : Testing
2018-01-21 23:04:30,615 INFO  -- This is info : Testing
2018-01-21 23:04:30,615 WARN  -- This is warn : Testing
2018-01-21 23:04:30,615 ERROR -- This is error : Testing
2018-01-21 23:04:30,615 FATAL -- This is fatal : Testing
```

7. Splunk & log4j
The below configuration will work for log4j and/or slf4j libraries and all we need to do is make sure `log4j.properties` is added along with the jar and change the conversionPattern as per splunk format. Hence log4j is known source type we don't have to create source type manually.
```xml
log4j.appender.A1.layout.conversionPattern=%d{ISO8601} logLevel=%p thread=\"%t\" category=%c %m%n
```

8. Additivity
If the logs are printing twice with same message then we are writing logs from rootLogger and from the configuration. We can mitigate this by setting Additivity=false for the parent class so parent and child class will be set to this.
```xml
#Setting log level & defining A1 for console log
log4j.logger.com.logger.Logging=INFO, CONSOLE
log4j.additivity.com.logger.Logging=false
```
