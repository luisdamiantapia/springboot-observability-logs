FROM maven:3.8.5-openjdk-17-slim as prebuild

RUN addgroup development; adduser --ingroup development --disabled-password developer

USER developer

WORKDIR /observability/app

COPY pom.xml .

RUN mvn dependency:go-offline -B

COPY src ./src

RUN mvn package -DskipTests

FROM openjdk:17-jdk-alpine

RUN addgroup development; adduser --ingroup development --disabled-password developer

USER developer

WORKDIR /app

COPY --from=prebuild /observability/app/target/logs-0.0.1-SNAPSHOT.jar .

COPY ../src/main/resources/application.properties ./config/application.properties

EXPOSE 8900

CMD ["java", "-jar", "logs-0.0.1-SNAPSHOT.jar", "--spring.config.location=config/application.properties"]