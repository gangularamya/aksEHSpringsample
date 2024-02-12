# Start with a base image containing Java runtime and Maven
FROM maven:3.8.4-openjdk-17-slim AS build

# The application's base directory
WORKDIR /usr/src/app

# Copy the pom.xml file to download dependencies
COPY pom.xml .

# Download the dependencies
RUN mvn dependency:go-offline -B

# Copy the source code
COPY src ./src

# Build the application
RUN mvn clean package

# Start with a base image containing Java runtime
FROM openjdk:17-jdk-slim

# Add a volume pointing to /tmp
VOLUME /tmp

# Make port 8080 available to the world outside this container
EXPOSE 8080

# Copy the application's jar file from the build image
COPY --from=build /usr/src/app/target/*.jar app.jar

# Run the jar file
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]