# Stage 1: Build the application
FROM maven:3.8.5-openjdk-21-slim AS build
WORKDIR /naming-server
COPY . .
RUN mvn clean package -DskipTests

# Stage 2: Create a lightweight runtime image
FROM openjdk:17-alpine
WORKDIR /naming-server
COPY --from=build /naming-server/target/*.jar app.jar

# Use a minimal command to run the application
ENTRYPOINT ["java", "-jar", "app.jar"]
