# Stage 1: Build the application
FROM maven:3.9.9-amazoncorretto-21-alpine AS build
WORKDIR /naming-server
COPY . .
RUN pwd
RUN ls -al
RUN mvn -f /naming-server/naming-server-service/pom.xml clean package -DskipTests

# Stage 2: Create a lightweight runtime image
FROM amazoncorretto:21-alpine
WORKDIR /naming-server
COPY --from=build /naming-server/naming-server-service/target/*.jar app.jar

# Use a minimal command to run the application
ENTRYPOINT ["java", "-jar", "app.jar"]
