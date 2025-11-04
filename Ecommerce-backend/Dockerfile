# Stage 1: build the application
FROM maven:3.9.6-eclipse-temurin-17 AS build
WORKDIR /app

# Copy only pom.xml to leverage caching for dependencies
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Copy source code
COPY src ./src

# Package
RUN mvn clean package -DskipTests -B

# Stage 2: run the application
FROM eclipse-temurin:17-jdk-alpine
WORKDIR /app

# Copy the built JAR from the build stage
COPY --from=build /app/target/*.jar app.jar

# If your application listens on a port (e.g., 8080), expose it
EXPOSE 8080

# Set environment variables if needed (database URL, etc)
ENV SPRING_PROFILES_ACTIVE=prod
# ENV SPRING_DATASOURCE_URL=<your-db-url>

ENTRYPOINT ["java","-jar","app.jar"]
