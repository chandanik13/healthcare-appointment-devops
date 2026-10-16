Healthcare Appointment Management System – DevOps Project

A Java-based Healthcare Appointment Management System integrated with DevOps practices for application development, automated testing, containerization, Kubernetes deployment, and monitoring using Prometheus and Grafana.

Table of Contents

Project Overview

Objectives

Technology Stack

System Architecture

Project Features

Project Structure

Prerequisites

Local Setup

Testing

Docker

Kubernetes Deployment

Jenkins CI/CD

Monitoring and Observability

API Endpoints

Results

Future Enhancements

Repository

Author

Project Overview

The Healthcare Appointment Management System is a REST API application developed using Java and Spring Boot. It provides appointment-related functionality and demonstrates how DevOps tools can support application building, testing, packaging, deployment, and monitoring.

The project integrates software development with operational practices to improve deployment consistency, testability, maintainability, and visibility into application health and performance.

Objectives

Develop a healthcare appointment management backend using Spring Boot.

Implement REST APIs for accessing appointment information.

Automate application builds and execute tests using Maven.

Use Git and GitHub for source code management.

Use Jenkins to demonstrate CI/CD pipeline automation.

Package the application into a Docker container.

Deploy the application on Kubernetes.

Collect application metrics using Prometheus.

Visualize application metrics through Grafana dashboards.

Technology Stack

Category

Technology

Programming Language

Java 21

Backend Framework

Spring Boot 3.5.7

Build Tool

Maven

Data Access

Spring Data JPA

Testing

JUnit, Spring Boot Test

Test Database

H2

Version Control

Git and GitHub

CI/CD

Jenkins

Containerization

Docker

Image Registry

Docker Hub

Container Orchestration

Kubernetes

Package Management

Helm

Metrics Collection

Prometheus

Visualization

Grafana

Application Monitoring

Spring Boot Actuator

System Architecture

The project follows this DevOps workflow:

Developer
    |
    v
Git and GitHub
    |
    v
Jenkins Pipeline
    |
    v
Maven Build and Tests
    |
    v
Docker Image
    |
    v
Docker Hub
    |
    v
Kubernetes Deployment
    |
    v
Healthcare Appointment Application
    |
    v
Spring Boot Actuator Metrics
    |
    v
Prometheus
    |
    v
Grafana Dashboard

Note: This diagram represents the intended end-to-end workflow. The level of automation between individual stages depends on the current Jenkins pipeline configuration.

Project Features

REST API endpoints for retrieving appointment information.

Java-based backend built with Spring Boot.

Maven-based application build and test workflow.

Automated testing support.

GitHub-based source code management.

Docker-based application packaging.

Kubernetes Deployment and Service configuration.

Prometheus metrics collection through Spring Boot Actuator.

Grafana dashboard for application monitoring.

JVM memory and process CPU metric visualization.

Project Structure

healthcare-appointment-devops/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── ... application source files
│   │   └── resources/
│   │       └── application.properties
│   └── test/
│       ├── java/
│       │   └── ... test source files
│       └── resources/
│           └── application.properties
├── Dockerfile
├── Jenkinsfile
├── k8s-deployment.yaml
├── healthcare-servicemonitor.yaml
├── pom.xml
├── mvnw
├── mvnw.cmd
└── README.md

The structure above is representative. Verify file names and directories against the actual repository before finalizing the documentation.

Prerequisites

Install the following tools before running the project:

JDK 21

Git

Docker Desktop

Kubernetes enabled in Docker Desktop

kubectl

Helm, for installing the monitoring stack

Jenkins

A web browser for accessing the application and dashboards

Local Setup

1. Clone the Repository

git clone https://github.com/chandanik13/healthcare-appointment-devops.git
cd healthcare-appointment-devops

2. Build the Application

On Windows Command Prompt or PowerShell:

.\mvnw.cmd clean package

On Linux or macOS:

./mvnw clean package

3. Run Automated Tests

Windows:

.\mvnw.cmd clean test

Linux or macOS:

./mvnw clean test

4. Run the Application

Windows:

.\mvnw.cmd spring-boot:run

Linux or macOS:

./mvnw spring-boot:run

The application may use port 8081 or another configured port depending on the local environment. Check the Spring Boot startup logs for the actual port.

Example endpoint:

http://localhost:8083/api/appointments

Use the port reported by your application if it differs from this example.

Testing

The project includes automated testing using the Maven test lifecycle and Spring Boot testing support.

Run:

.\mvnw.cmd clean test

The test suite verifies the behavior covered by the implemented test cases. Maven test reports are generally generated under:

target/surefire-reports/

Do not assume all possible appointment scenarios are tested; refer to the actual test classes and results.

Docker

Docker packages the application into a container image to support consistent deployment.

Build the Application

.\mvnw.cmd clean package

Build the Docker Image

Run from the project root:

docker build -t chandanik/healthcare-appointment:latest .

Run the Container

docker run --rm -p 8081:8081 chandanik/healthcare-appointment:latest

Ensure that the container's application port matches the port configured in the Dockerfile and Spring Boot application.

Docker Hub

Repository:

https://hub.docker.com/r/chandanik/healthcare-appointment/tags

Before pushing an image, authenticate using Docker's interactive login command and ensure you have permission to publish to the repository.

Kubernetes Deployment

The application can be deployed using the Kubernetes configuration provided in the repository.

Apply the Deployment

kubectl apply -f k8s-deployment.yaml

Check Pods

kubectl get pods

Check Deployments

kubectl get deployments

Check Services

kubectl get services

Check Deployment Status

kubectl rollout status deployment/healthcare-appointment

Access the Application

If the Kubernetes Service exposes port 8081, forward a local port:

kubectl port-forward service/healthcare-appointment 8082:8081

Keep the terminal running and access:

http://localhost:8082/api/appointments

The port-forward command is intended for local testing, not public internet access.

Jenkins CI/CD

Jenkins is used to demonstrate automation in the software delivery workflow.

The project contains a Jenkinsfile that can define pipeline stages for building, testing, packaging, and other deployment tasks.

Typical pipeline stages include:

Source code checkout.

Dependency resolution.

Application compilation and packaging.

Automated test execution.

Docker image build.

Image publishing to Docker Hub.

Kubernetes deployment.

Only describe stages as automated when they are actually configured and successfully executed in Jenkins.

Monitoring and Observability

The project uses Prometheus and Grafana to monitor application metrics.

Prometheus

Prometheus collects metrics exposed by the Spring Boot Actuator Prometheus endpoint:

/actuator/prometheus

The configured ServiceMonitor enables Prometheus to discover and scrape the application's metrics endpoint.

An example Prometheus query for checking target availability is:

up{job="healthcare-appointment"}

A value of 1 indicates that the selected target is currently being scraped successfully.

Grafana

Grafana is used to visualize metrics collected by Prometheus.

The project dashboard is named:

Healthcare Appointment Monitoring

The dashboard includes panels for process CPU usage and JVM heap memory usage.

Example JVM heap memory query:

sum(jvm_memory_used_bytes{
  area="heap",
  job="healthcare-appointment"
}) / 1024 / 1024

This query displays the summed JVM heap memory usage in MiB for matching series.

Accessing Monitoring Tools

The following URLs are examples for a local Docker Desktop Kubernetes setup. Port forwarding may be required.

Tool

Local URL

Application

http://localhost:8082

Prometheus

http://localhost:9090

Grafana

http://localhost:3000

Keep the corresponding port-forward terminals running while accessing these services.

API Endpoints

The following GET endpoints have been verified during project testing.

Method

Endpoint

Description

GET

/api/appointments

Retrieve the appointment list

GET

/api/appointments/{id}

Retrieve an appointment by ID

Example request:

curl http://localhost:8083/api/appointments

Example request by ID:

curl http://localhost:8083/api/appointments/1

Replace the port with the port on which the application is running. These examples assume that appointment ID 1 exists in the active database.

Results

The following outcomes were observed during project development and testing:

The application successfully returned appointment data through its REST API.

Maven automated tests completed successfully during development.

The Kubernetes application deployment reached the Running state.

The Prometheus target for the healthcare appointment application was observed as UP.

The Prometheus query up{job="healthcare-appointment"} returned 1.

A Grafana dashboard was created to visualize application CPU and JVM memory metrics.

Jenkins was configured and a preliminary pipeline was executed successfully.

These results demonstrate the implemented functionality and monitoring setup. Full end-to-end pipeline automation should be claimed only after all relevant Jenkins stages have been verified.

Future Enhancements

Implement appointment creation, updating, and cancellation APIs if not already present.

Add authentication and role-based authorization.

Integrate a production-grade database where required.

Add comprehensive unit and integration tests.

Complete automated Docker image publishing and Kubernetes deployment through Jenkins.

Introduce automated deployment verification and rollback.

Configure alerts for application health and resource thresholds.

Add centralized logging and distributed tracing.

Deploy the application and monitoring stack to a cloud environment.

Improve documentation and include verified screenshots of each deployment stage.

Repository

GitHub:
https://github.com/chandanik13/healthcare-appointment-devops

Docker Hub:
https://hub.docker.com/r/chandanik/healthcare-appointment/tags
