# 1: build con Maven
FROM maven:3.9-eclipse-temurin-17 AS build

WORKDIR /app

COPY pom.xml .
COPY src ./src

RUN mvn clean package -DskipTests

# Ajusta el nombre del jar final según target/
# Ejemplo: Springboot-API-REST-0.0.1-SNAPSHOT.jar
# Compruébalo en la carpeta target después de compilar.

# 2: imagen final más liviana
FROM eclipse-temurin:17-jdk-alpine

WORKDIR /app

COPY --from=build /app/target/*.jar app.jar

# Variables de entorno típicas para DB; ya las usamos en application.properties
ENV DB_ENDPOINT=localhost \
    DB_PORT=3306 \
    DB_NAME=test \
    DB_USERNAME=root \
    DB_PASSWORD=changeme

EXPOSE 8080

CMD ["java", "-jar", "app.jar"]