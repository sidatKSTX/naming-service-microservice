# Stage 1: Build the application
#FROM maven:3.9.9-amazoncorretto-21-alpine AS build
#WORKDIR /naming-server
#COPY . .
#RUN mvn -f /naming-server/naming-server-service/pom.xml clean package -DskipTests

# Stage 2: Create a lightweight runtime image
#FROM amazoncorretto:21-alpine
#WORKDIR /naming-server
#COPY --from=build /naming-server/naming-server-service/target/*.jar naming-server-service.jar

# Use a minimal command to run the application
#ENTRYPOINT ["java", "-jar", "naming-server-service.jar"]


# Stage 1: Build the application
FROM maven:3.9.9-amazoncorretto-21-alpine AS build
WORKDIR /app

# Copy only required files first for efficient caching
COPY naming-server-service/pom.xml naming-server-service/
RUN mvn -f naming-server-service/pom.xml dependency:go-offline

# Copy the rest of the project
COPY . .
RUN mvn -f naming-server-service/pom.xml clean package -DskipTests

# Stage 2: Create a minimal runtime image
FROM amazoncorretto:21-alpine AS runtime
WORKDIR /app

# Copy only the built jar
COPY --from=build /app/naming-server-service/target/naming-server-service.jar naming-server-service.jar

# Run the application with minimal memory usage
ENTRYPOINT ["java", "-jar", "-XX:+UseContainerSupport", "-XX:MaxRAMPercentage=80", "naming-server-service.jar"]
