# Dockerized Java Application
This project uses Docker to containerize a Java application built with Maven and package it into a lightweight runtime image using a multi-stage Docker build.

## Project Overview
This project demonstrates:
- Build a Java application using Maven
- Create an optimized Docker image using a multi-stage Dockerfile
- Run Java application inside a Docker container
- Expose the application through a host port
- Push the Docker image to Docker Hub

## Prerequisites
- Docker installed
- Docker Hub account
- Java application source code
- Maven project (pom.xml)

## Technology used
- Java 17
- Maven 3.8.4
- Docker
- Eclipse Temurin JDK 17

## Dockerfile Explanation
- The application uses a multi-stage Docker build: Build Stage

FROM maven:3.8.4-openjdk-17-slim AS build

WORKDIR /app

COPY . .

RUN mvn clean package -DskipTests

## This stage:
- Uses a Maven image with OpenJDK 17
- Copies the source code into the container
- Builds the application
- Generates a JAR file inside the target/ directory

## Runtime Stage
FROM eclipse-temurin:17-jdk-jammy

WORKDIR /app

COPY --from=build /app/target/*.jar app.jar

EXPOSE 8081

ENTRYPOINT ["java", "-jar", "app.jar"]

## This stage:
- Uses a lightweight Java 17 runtime image
- Copies only the generated JAR file
- Exposes port 8081
- Starts the application automatically

## Build the Docker Image
- Run the following command from the project root directory: docker build -t jeffersonohis1/java-image:v1.0.0 .
- Verify the image: docker images

## Run the Container
- Start the container: docker run -d --name my-java-app-container -p 5050:8081 jeffersonohis1/java-image:v1.0.0

## Port Mapping
- Host Machine: 5050
- Container: 8081

## Access the Application 
- Open your browser and navigate to: http://localhost:5050

## Verify the Container
- Check running containers: docker ps
- View container logs: docker logs my-java-app-container
- Inspect the container: docker inspect my-java-app-container

## Stop and Remove Container
- Stop the container: docker stop my-java-app-container
- Remove the container: docker rm my-java-app-container

## Push Image to Docker Hub
- Login to Docker Hub: docker login
- Push the image: docker push jeffersonohis1/java-image:v1.0.0
- Pull the image on any machine: docker pull jeffersonohis1/java-image:v1.0.0
- Run Pulled Image: docker run -d --name my-java-app-container -p 5050:8081 jeffersonohis1/java-image:v1.0.0

## Project Structure
├── src/
├── target/
├── Dockerfile
├── pom.xml
└── README.md

## Author
Jefferson Ohis
DevOps Engineer | AWS Certified Cloud Practitioner
GitHub: https://github.com/Jefferson-ohis1
Docker Hub: https://hub.docker.com/u/jeffersonohis1
