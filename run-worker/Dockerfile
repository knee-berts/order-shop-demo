FROM openjdk:15-jdk-slim as bulid
WORKDIR application

COPY mvnw .
COPY .mvn .mvn
COPY pom.xml .
COPY src src

RUN ./mvnw  install -DskipTests

RUN cp target/*.jar app.jar
RUN java -Djarmode=layertools -jar app.jar extract

FROM openjdk:15-jdk-slim
WORKDIR application
COPY --from=bulid application/dependencies/ ./
COPY --from=bulid application/spring-boot-loader/ ./
COPY --from=bulid application/snapshot-dependencies/ ./
COPY --from=bulid application/application/ ./
ENTRYPOINT ["java", "org.springframework.boot.loader.JarLauncher"]