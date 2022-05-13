# Spring Microservices


## What will you learn?

- Docker
- Kubernetes
- Spring Boot 2.4.x+ & Spring Cloud 2020.x+
  - Service Registry using Eureka Naming Server
  - Load Balancing with Spring Cloud LoadBalancer (replacing Ribbon)
  - API Gateway with Spring Cloud Gateway (instead of Zuul)
  - Circuit Breaker with Resilience4j (instead of Hystrix)
  - Distributed Tracing with Zipkin
  - Asynchronous Communication using Rabbit MQ

#### Microservices with Spring Cloud - V2

-  Step 01 - Setting up Limits Microservice
-  Step 02 - Creating a hard coded limits service
-  Step 03 - Enhance limits service to pick up configuration from application properties
-  Step 04 - Setting up Spring Cloud Config Server
-  Step 05 - Installing Git and Creating Local Git Repository
-  Step 06 - Connect Spring Cloud Config Server to Local Git Repository
-  Step 07 - Connect Limits Service to Spring Cloud Config Server
-  Step 08 - Configuring Profiles for Limits Service
-  Step 09 - Introduction to Currency Conversion and Currency Exchange Microservices
-  Step 10 - Setting up Currency Exchange Microservice
-  Step 11 - Create a simple hard coded currency exchange service
-  Step 12 - Setting up Dynamic Port in the the Response
-  Step 13 - Configure JPA and Initialized Data
-  Step 14 - Create a JPA Repository
-  Step 15 - Setting up Currency Conversion Microservice
-  Step 16 - Creating a service for currency conversion
-  Step 17 - Invoking Currency Exchange Microservice from Currency Conversion Microservice
-  Step 18 - Using Feign REST Client for Service Invocation
-  Step 19 - Understand Naming Server and Setting up Eureka Naming Server
-  Step 20 - Connect Currency Conversion Microservice & Currency Exchange Microservice to Eureka
-  Step 21 - Load Balancing with Eureka, Feign & Spring Cloud LoadBalancer
-  Step 22 - Setting up Spring Cloud API Gateway
-  Step 23 - Enabling Discovery Locator with Eureka for Spring Cloud Gateway
-  Step 24 - Exploring Routes with Spring Cloud Gateway
-  Step 25 - Implementing Spring Cloud Gateway Logging Filter
-  Step 26 - Getting started with Circuit Breaker - Resilience4j
-  Step 27 - Playing with Resilience4j - Retry and Fallback Methods
-  Step 28 - Playing with Circuit Breaker Features of Resilience4j
-  Step 29 - Exploring Rate Limiting and BulkHead Features of Resilience4j

#### Docker with Microservices using Spring Boot and Spring Cloud - V2

-  Step 00 - Match made in Heaven - Docker and Microservices
-  Step 01 - Installing Docker - Docker
-  Step 02 - Your First Docker Usecase - Deploy a Spring Boot Application
-  Step 03 - Important Docker Concepts - Registry, Repository, Tag, Image and Container
-  Step 04 - Playing with Docker Images and Containers
-  Step 05 - Understanding Docker Architecture - Docker Client, Docker Engine
-  Step 06 - Why is Docker Popular
-  Step 07 - Playing with Docker Images
-  Step 08 - Playing with Docker Containers
-  Step 09 - Playing with Docker Commands - stats, system
-  Step 10 - Introduction to Distributed Tracing
-  Step 11 - Launching Zipkin Container using Docker
-  Step 12 - Connecting Currency Exchange Microservice with Zipkin
-  Step 13 - Connecting Currency Conversion Microservice and API Gateway with Zipkin
-  Step 14 - Getting Setup with Microservices for Creating Container Images
-  Step 15 - Creating Container Image for Currency Exchange Microservice
-  Step 16 - Getting Started with Docker Compose - Currency Exchange Microservice
-  Step 17 - Running Eureka Naming Server with Docker Compose
-  Step 18 - Running Currency Conversion Microservice with Docker Compose
-  Step 19 - Running Spring Cloud API Gateway with Docker Compose
-  Step 20 - Running Zipkin with Docker Compose
-  Step 21 - Running Zipkin and RabbitMQ with Docker Compose

#### Kubernetes with Microservices using Docker, Spring Boot and Spring Cloud - V2

-  Step 00 - Docker, Kubernetes and Microservices - Made for each other
-  Step 01 - Getting Started with Docker, Kubernetes and Google Kubernetes Engine
-  Step 02 - Creating Google Cloud Account
-  Step 03 - Creating Kubernetes Cluster with Google Kubernete Engine (GKE)
-  Step 04 - Review Kubernetes Cluster and Learn Few Fun Facts about Kubernetes
-  Step 05 - Deploy Your First Spring Boot Application to Kubernetes Cluster
-  Step 06 - Quick Look at Kubernetes Concepts - Pods, Replica Sets and Deployment
-  Step 07 - Understanding Pods in Kubernetes
-  Step 08 - Understanding ReplicaSets in Kubernetes
-  Step 09 - Understanding Deployment in Kubernetes
-  Step 10 - Quick Review of Kubernetes Concepts - Pods, Replica Sets and Deployment
-  Step 11 - Understanding Services in Kubernetes
-  Step 12 - Quick Review of GKE on Google Cloud Console 
-  Step 13 - Understanding Kubernetes Architecture - Master Node and Nodes
-  Step 14 - Setup Currency Exchange & Currency Conversion Microservices - K8S versions
-  Step 15 - Create Container images for Currency Exchange & Currency Conversion Microservices
-  Step 16 - Deploy Microservices to Kubernetes & Understand Service Discovery
-  Step 17 - Creating Declarative Configuration Kubernetes YAML for Microservices
-  Step 18 - Clean up Kubernetes YAML for Microservices
-  Step 19 - Enable Logging and Tracing APIs in Google Cloud Platform
-  Step 20 - Deploying Microservices using Kubernetes YAML Configuration
-  Step 21 - Playing with Kubernetes Declarative YAML Configuration
-  Step 22 - Creating Environment Variables to enable Microservice Communication
-  Step 23 - Understanding Centralized Configuration in Kubernetes - Config Maps
-  Step 24 - Exploring Centralized Logging and Monitoring in GKE
-  Step 25 - Exploring Microservices Deployments with Kubernetes
-  Step 26 - Configuring Liveness and Readiness Probes for Microservices with K8S
-  Step 27 - Autoscaling Microservices with Kubernetes
-  Step 28 - Delete Kubernetes Cluster and Thank You!

## URLS

|     Application       |     URL          |
| ------------- | ------------- |
| Limits Service | http://localhost:8080/limits http://localhost:8080/actuator/refresh  (POST)|
|Spring Cloud Config Server| http://localhost:8888/limits-service/default http://localhost:8888/limits-service/dev |
|  Currency Converter Service - Direct Call| http://localhost:8100/currency-converter/from/USD/to/INR/quantity/10|
|  Currency Converter Service - Feign| http://localhost:8100/currency-converter-feign/from/EUR/to/INR/quantity/10000|
| Currency Exchange Service | http://localhost:8000/currency-exchange/from/EUR/to/INR http://localhost:8001/currency-exchange/from/USD/to/INR|
| Eureka | http://localhost:8761/|
| Zuul - Currency Exchange & Exchange Services | http://localhost:8765/currency-exchange-service/currency-exchange/from/EUR/to/INR http://localhost:8765/currency-conversion-service/currency-converter-feign/from/USD/to/INR/quantity/10|
| Zipkin | http://localhost:9411/zipkin/ |
| Spring Cloud Bus Refresh | http://localhost:8080/actuator/bus-refresh (POST)|

## Ports

|     Application       |     Port          |
| ------------- | ------------- |
| Limits Service | 8080, 8081, ... |
| Spring Cloud Config Server | 8888 |
|  |  |
| Currency Exchange Service | 8000, 8001, 8002, ..  |
| Currency Conversion Service | 8100, 8101, 8102, ... |
| Netflix Eureka Naming Server | 8761 |
| Netflix Zuul API Gateway Server | 8765 |
| Zipkin Distributed Tracing Server | 9411 |


## Ranga Local Instructions

```
brew install rabbitmq
/usr/local/sbin/rabbitmq-server
curl -X POST localhost:8080/actuator/refresh
curl -X POST localhost:8080/actuator/bus-refresh
```

## Zipkin Installation

Quick Start Page
- https://zipkin.io/pages/quickstart

Downloading Zipkin Jar
- https://search.maven.org/remote_content?g=io.zipkin.java&a=zipkin-server&v=LATEST&c=exec

Command to run
```
RABBIT_URI=amqp://localhost java -jar zipkin-server-2.12.9-exec.jar 
```

## More Reading about Microservices
- Design and Governance of Microservices
    - https://martinfowler.com/microservices/
- 12 Factor App 
    - https://12factor.net/
    - https://dzone.com/articles/the-12-factor-app-a-java-developers-perspective
- Spring Cloud
    - http://projects.spring.io/spring-cloud/
	
	
### Troubleshooting
- Refer our TroubleShooting Guide - https://github.com/in28minutes/in28minutes-initiatives/tree/master/The-in28Minutes-TroubleshootingGuide-And-FAQ

## Youtube Playlists - 500+ Videos

[Click here - 30+ Playlists with 500+ Videos on Spring, Spring Boot, REST, Microservices and the Cloud](https://www.youtube.com/user/rithustutorials/playlists?view=1&sort=lad&flow=list)

## Keep Learning in28Minutes

in28Minutes is creating amazing solutions for you to learn Spring Boot, Full Stack and the Cloud - Docker, Kubernetes, AWS, React, Angular etc. - [Check out all our courses here](https://github.com/in28minutes/learn)

![in28MinutesLearningRoadmap-July2019.png](https://github.com/in28minutes/in28Minutes-Course-Roadmap/raw/master/in28MinutesLearningRoadmap-July2019.png)