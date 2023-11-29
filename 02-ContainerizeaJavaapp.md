
# Containerize a Java Application

## Overview
This guide covers the steps to containerize the Flight Booking System for Airline Reservations using Docker.

## Steps

### 1. Clone the Java Application
Clone the repository and change to the Airlines project folder:
```bash
git clone https://github.com/dguncetdci/containerize-and-deploy-Java-app-to-Azure.git
cd containerize-and-deploy-Java-app-to-Azure/Project/Airlines
```

**Note:** If the Azure Kubernetes Service creation is complete, continue in the same CLI tab. If it's still running, open a new tab for these steps.

### 2. Build the Application (Optional)
This step is optional and requires Java & Maven. It gives a sense of building the app without Docker.
```bash
mvn clean install
```

**Note:** The `mvn clean install` command illustrates the challenges of not using Docker multi-stage builds. This step is optional, and you can proceed without executing it.

### 3. Construct a Docker File
In the project root, create a Dockerfile:
```bash
vi Dockerfile
```

Add the following contents to the Dockerfile, then save and exit:
```Dockerfile
# Build stage
FROM maven:3.6.0-jdk-11-slim AS build
WORKDIR /build
COPY pom.xml .
COPY src ./src
COPY web ./web
RUN mvn clean package

# Package stage
FROM tomcat:8.5.72-jre11-openjdk-slim
COPY tomcat-users.xml /usr/local/tomcat/conf
COPY --from=build /build/target/*.war /usr/local/tomcat/webapps/FlightBookingSystemSample.war
EXPOSE 8080
CMD ["catalina.sh", "run"]
```

**Note:** Alternatively, you can use the `Dockerfile_Solution` in the root of your project.

---

Proceed to build and deploy the containerized Java application following the above steps.
