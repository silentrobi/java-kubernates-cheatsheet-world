# Docker Container Springboot
https://spring.io/guides/topicals/spring-boot-docker/
# Better DockerFile
**What is Multi stage docker file?**

https://www.youtube.com/watch?v=hmpUegtHG9s&ab_channel=SchoolofDevops

```dockerfile
##-----------build stage--------------
FROM amazoncorretto:17-alpine-jdk as builder
WORKDIR app-builder

COPY target/*.jar application.jar

#This command layers parts of the application based on how likely they are to change between application builds.
RUN java -Djarmode=layertools -jar application.jar extract


##-----------run stage--------------
FROM amazoncorretto:17-alpine-jdk as run
WORKDIR app

# Copying all layered parts of the application from build stage.
COPY --from=builder app-builder/dependencies/ ./
COPY --from=builder app-builder/spring-boot-loader/ ./
COPY --from=builder app-builder/snapshot-dependencies/ ./
COPY --from=builder app-builder/application/ ./

COPY run.sh /app/run.sh
RUN chmod +x /app/run.sh

EXPOSE 8080

# Bootstrap the java application.
ENTRYPOINT ["/app/run.sh"]
```
run.sh
```sh
#!/bin/sh

# ${JAVA_OPTS} is for passing inject environment variables into the Java process at runtime.
# e.g.: docker run -p 8080:8080 -e "JAVA_OPTS=-Ddebug -Xmx128m" myorg/myapp
#
# org.springframework.boot.loader.JarLauncher --> The Spring Boot fat JarLauncher is extracted from the JAR into the image, so it can be used to start the application without hard-coding the main application class.
exec java ${JAVA_OPTS} org.springframework.boot.loader.JarLauncher
```

**Another way of doing the same thing:**
```dockerfile
FROM amazoncorretto:17-alpine-jdk as builder
WORKDIR /workspace/app

COPY mvnw .
COPY .mvn .mvn
COPY pom.xml .
COPY src src

RUN ./mvnw install -DskipTests
RUN mkdir -p target/dependency && (cd target/dependency; jar -xf ../*.jar)

FROM amazoncorretto:17-alpine-jdk as run
WORKDIR app
ARG DEPENDENCY=/workspace/app/target/dependency
COPY --from=build ${DEPENDENCY}/BOOT-INF/lib /app/lib
COPY --from=build ${DEPENDENCY}/META-INF /app/META-INF
COPY --from=build ${DEPENDENCY}/BOOT-INF/classes /app

COPY run.sh /app/run.sh
RUN chmod +x /app/run.sh

EXPOSE 8080

# Bootstrap the java application.
ENTRYPOINT ["/app/run.sh"]
```
Let's see how it looks like inside container.
```sh
/app # find . | sed -e "s/[^-][^\/]*\// |/g" -e "s/|\([^ ]\)/|-\1/"
.
 |-run.sh
 |-BOOT-INF
 | |-lib
 | | |-spring-boot-autoconfigure-3.0.5.jar
 | | |-jackson-datatype-jsr310-2.14.2.jar
 | | |-log4j-api-2.19.0.jar
 | | |-tomcat-embed-core-10.1.7.jar
 | | |-tomcat-embed-el-10.1.7.jar
 | | |-spring-boot-3.0.5.jar
 | | |-logback-classic-1.4.6.jar
 | | |-spring-boot-jarmode-layertools-3.0.5.jar
 | | |-lombok-1.18.24.jar
 | | |-spring-expression-6.0.7.jar
 | | |-jackson-core-2.14.2.jar
 | | |-jackson-databind-2.14.2.jar
 | | |-spring-jcl-6.0.7.jar
 | | |-snakeyaml-1.33.jar
 | | |-spring-core-6.0.7.jar
 | | |-spring-beans-6.0.7.jar
 | | |-jackson-datatype-jdk8-2.14.2.jar
 | | |-jackson-annotations-2.14.2.jar
 | | |-jul-to-slf4j-2.0.7.jar
 | | |-spring-web-6.0.7.jar
 | | |-slf4j-api-2.0.7.jar
 | | |-spring-webmvc-6.0.7.jar
 | | |-logback-core-1.4.6.jar
 | | |-micrometer-commons-1.10.5.jar
 | | |-spring-context-6.0.7.jar
 | | |-spring-aop-6.0.7.jar
 | | |-jakarta.annotation-api-2.1.1.jar
 | | |-tomcat-embed-websocket-10.1.7.jar
 | | |-jackson-module-parameter-names-2.14.2.jar
 | | |-micrometer-observation-1.10.5.jar
 | | |-log4j-to-slf4j-2.19.0.jar
 | | |-user-service-service-1.0-SNAPSHOT.jar
 | |-classes
 | | |-application.properties
 | | |-org
 | | | |-example
 | | | | |-userservicerestapplication
 | | | | | |-controller
 | | | | | | |-UserController.class
 | | | | | |-UserServiceRestApplication.class
 | |-classpath.idx
 | |-layers.idx
 |-META-INF
 | |-maven
 | | |-org.example
 | | | |-user-service-rest-application
 | | | | |-pom.properties
 | | | | |-pom.xml
 | |-MANIFEST.MF
 |-org
 | |-springframework
 | | |-boot
 | | | |-loader
 | | | | |-data
 | | | | | |-RandomAccessDataFile.class
 | | | | | |-RandomAccessData.class
 | | | | | |-RandomAccessDataFile$DataInputStream.class
 | | | | | |-RandomAccessDataFile$FileAccess.class
 | | | | |-WarLauncher.class
 | | | | |-PropertiesLauncher$ArchiveEntryFilter.class
 | | | | |-ClassPathIndexFile.class
 | | | | |-jarmode
 | | | | | |-JarMode.class
 | | | | | |-TestJarMode.class
 | | | | | |-JarModeLauncher.class
 | | | | |-MainMethodRunner.class
 | | | | |-LaunchedURLClassLoader$UseFastConnectionExceptionsEnumeration.class
 | | | | |-util
 | | | | | |-SystemPropertyUtils.class
 | | | | |-jar
 | | | | | |-JarFileEntries$1.class
 | | | | | |-JarFileEntries$EntryIterator.class
 | | | | | |-JarFileEntries.class
 | | | | | |-CentralDirectoryEndRecord$Zip64Locator.class
 | | | | | |-JarEntry.class
 | | | | | |-CentralDirectoryEndRecord.class
 | | | | | |-AbstractJarFile.class
 | | | | | |-JarFile$1.class
 | | | | | |-AsciiBytes.class
 | | | | | |-JarURLConnection$JarEntryName.class
 | | | | | |-FileHeader.class
 | | | | | |-JarEntryFilter.class
 | | | | | |-JarFileWrapper.class
 | | | | | |-JarURLConnection$1.class
 | | | | | |-JarFileEntries$Zip64Offsets.class
 | | | | | |-CentralDirectoryEndRecord$Zip64End.class
 | | | | | |-JarFile.class
 | | | | | |-CentralDirectoryParser.class
 | | | | | |-CentralDirectoryVisitor.class
 | | | | | |-JarFile$JarEntryEnumeration.class
 | | | | | |-JarFileEntries$Offsets.class
 | | | | | |-JarURLConnection.class
 | | | | | |-Bytes.class
 | | | | | |-ZipInflaterInputStream.class
 | | | | | |-AbstractJarFile$JarFileType.class
 | | | | | |-CentralDirectoryFileHeader.class
 | | | | | |-StringSequence.class
 | | | | | |-JarEntryCertification.class
 | | | | | |-JarFileEntries$ZipOffsets.class
 | | | | | |-Handler.class
 | | | | |-LaunchedURLClassLoader$DefinePackageCallType.class
 | | | | |-PropertiesLauncher.class
 | | | | |-LaunchedURLClassLoader.class
 | | | | |-JarLauncher.class
 | | | | |-PropertiesLauncher$PrefixMatchingArchiveFilter.class
 | | | | |-Launcher.class
 | | | | |-archive
 | | | | | |-ExplodedArchive$EntryIterator.class
 | | | | | |-JarFileArchive$EntryIterator.class
 | | | | | |-JarFileArchive.class
 | | | | | |-Archive$EntryFilter.class
 | | | | | |-ExplodedArchive$ArchiveIterator.class
 | | | | | |-ExplodedArchive$FileEntry.class
 | | | | | |-JarFileArchive$AbstractIterator.class
 | | | | | |-JarFileArchive$JarFileEntry.class
 | | | | | |-JarFileArchive$NestedArchiveIterator.class
 | | | | | |-Archive.class
 | | | | | |-ExplodedArchive$SimpleJarFileArchive.class
 | | | | | |-Archive$Entry.class
 | | | | | |-ExplodedArchive.class
 | | | | | |-ExplodedArchive$AbstractIterator.class
 | | | | |-PropertiesLauncher$ClassPathArchives.class
 | | | | |-ExecutableArchiveLauncher.class
```
