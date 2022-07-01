# spring-microservices interview guide
- https://github.com/jdbirla/JD_Spring_Microservices/tree/master/Documents
- [Restful] https://github.com/jdbirla/JD_Spring_Microservices/blob/master/restful-web-services/SM_Restful_Web_Services_Detail.md
- [Microservice] https://github.com/jdbirla/JD_Spring_Microservices/blob/master/microservices/V2/README.md

## High Level Rest APIs problems and evolution
   1. Intruducing ``` Creating HelloWorld Rest API``` [Solution for Problem (Limit-service): Rest API ]
   2. Intruducing ``` Return a Bean``` [Solution for Problem (Limit-service): Restrun a bean in response]
   3. Intruducing ``` Path Variable``` [Solution for Problem (Limit-service): Path Variable for request paramter]
   4. Intruducing ``` User Bean and User Service``` [Solution for Problem (Limit-service): Creating User bean and User service]
   5. Intruducing ```  Post Method return correct HTTP Status code and Location``` [Solution for Problem (Limit-service): Post Method return correct HTTP Status code and Location using ResponseEntity]
   6. Intruducing ```  Generic Exeption Handling``` [Solution for Problem (Limit-service): Genric excpetion handling using ResponseEntityExceptionHandler]
   7. Intruducing ```  Delete Method``` [Solution for Problem (Limit-service): Create Delete method]
   8. Intruducing ```  Validation ``` [Solution for Problem (Limit-service): validaiton using javax validaiton @Valid]
   9. Intruducing ``` HATEOAS``` [Solution for Problem (Limit-service): Creating HATEOAS for get reques ]
   10. Intruducing ``` i18 ``` [Solution for Problem (Limit-service): Implemention i18 using MessageSource ]
   11. Intruducing ``` Content negotiation ``` [Solution for Problem (Limit-service): Chnage the formate using jackson for XML ]
   12. Intruducing ``` Swagger ``` [Solution for Problem (Limit-service): Swagger documentaiton for APIs ]
   13. Intruducing ``` Spring Boot Actuatot ``` [Solution for Problem (Limit-service): Monitoring using Spring Boot Actuator ]
   14. Intruducing ``` HAL Explorer ``` [Solution for Problem (Limit-service): Visualizing APIs with HAL explorer]
   15. Intruducing ``` Static / Dynamic Filtering ``` [Solution for Problem (Limit-service): Static filtering //@JsonIgnore and dynammic filtering MappingJacksonValue  ]
   16. Intruducing ``` Versioning ``` [Solution for Problem (Limit-service): version using URI/param/header/produces ]
   17. Intruducing ``` Security ``` [Solution for Problem (Limit-service): Spring Security ]
   18. Intruducing ``` JPA ``` [Solution for Problem (Limit-service): Connecitng to JPA]
   
---

Response Status
200 - SUCCESS
404 - RESOURCE NOT FOUND
400 - BAD REQUEST
201 - CREATED
401 - UNAUTHORIZED
500 - SERVER ERROR

```java
package com.jd.rest.webservices.restfulwebservices.user;

import static org.springframework.hateoas.server.mvc.WebMvcLinkBuilder.linkTo;
import static org.springframework.hateoas.server.mvc.WebMvcLinkBuilder.methodOn;

import java.net.URI;
import java.util.List;

import javax.validation.Valid;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.hateoas.EntityModel;
import org.springframework.hateoas.server.mvc.WebMvcLinkBuilder;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.servlet.support.ServletUriComponentsBuilder;

@RestController
public class UserResource {

	@Autowired
	private UserDaoService service;

	@GetMapping("/users")
	public List<User> retrieveAllUsers() {
		return service.findAll();
	}

	@GetMapping("/users/{id}")
	public EntityModel<User> retrieveUser(@PathVariable int id) {
		User user = service.findOne(id);

		if (user == null)
			throw new UserNotFoundException("id-" + id);

		EntityModel<User> model = EntityModel.of(user);

		WebMvcLinkBuilder linkToUsers = linkTo(methodOn(this.getClass()).retrieveAllUsers());
		model.add(linkToUsers.withRel("all-users"));

		return model;
	}

	//
	// input - details of user
	// output - CREATED & Return the created URI
	@PostMapping("/users")
	public ResponseEntity<Object> createUser(@Valid @RequestBody User user) {
		User savedUser = service.save(user);
		// created
		// user/{id} saveUser.getId()
		URI location = ServletUriComponentsBuilder.fromCurrentRequest().path("/{id}").buildAndExpand(savedUser.getId())
				.toUri();
		return ResponseEntity.created(location).build();

	}

	@DeleteMapping("/users/{id}")
	public void deleteUser(@PathVariable int id) {
		User user = service.deleteById(id);

		if (user == null)
			throw new UserNotFoundException("id-" + id);
	}

}
```

- Ubsing JPA

```java
package com.jd.rest.webservices.restfulwebservices.user;

import static org.springframework.hateoas.server.mvc.WebMvcLinkBuilder.linkTo;
import static org.springframework.hateoas.server.mvc.WebMvcLinkBuilder.methodOn;

import java.net.URI;
import java.util.List;
import java.util.Optional;

import javax.validation.Valid;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.hateoas.EntityModel;
import org.springframework.hateoas.server.mvc.WebMvcLinkBuilder;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.servlet.support.ServletUriComponentsBuilder;

@RestController
public class UserJPAResource {

	
	@Autowired
	private UserRepository userRepository;
	
	
	@Autowired
	private PostRepository postRepository;

	@GetMapping("/jpa/users")
	public List<User> retrieveAllUsers() {
		return userRepository.findAll();
	}

	@GetMapping("/jpa/users/{id}")
	public EntityModel<User> retrieveUser(@PathVariable int id) {
		Optional<User> user = userRepository.findById(id);

		if (!user.isPresent())
			throw new UserNotFoundException("id-" + id);

		EntityModel<User> model = EntityModel.of(user.get());

		WebMvcLinkBuilder linkToUsers = linkTo(methodOn(this.getClass()).retrieveAllUsers());
		model.add(linkToUsers.withRel("all-users"));

		return model;
	}

	//
	// input - details of user
	// output - CREATED & Return the created URI
	@PostMapping("/jpa/users")
	public ResponseEntity<Object> createUser(@Valid @RequestBody User user) {
		User savedUser = userRepository.save(user);
		// created
		// user/{id} saveUser.getId()
		URI location = ServletUriComponentsBuilder.fromCurrentRequest().path("/{id}").buildAndExpand(savedUser.getId())
				.toUri();
		return ResponseEntity.created(location).build();

	}

	@DeleteMapping("/jpa/users/{id}")
	public void deleteUser(@PathVariable int id) {
	 userRepository.deleteById(id);
	}
	
	@GetMapping("/jpa/users/{id}/posts")
	public List<Post> retrieveAllPostsByUser(@PathVariable int id) {
		Optional<User> user = userRepository.findById(id);

		if (!user.isPresent())
			throw new UserNotFoundException("id-" + id);
		
		return user.get().getPosts();
		
	}
	
	@PostMapping("/jpa/users/{id}/posts")
	public ResponseEntity<Object> createPost(@PathVariable int id, @RequestBody Post post) {
		Optional<User> userOptional = userRepository.findById(id);

		if (!userOptional.isPresent())
			throw new UserNotFoundException("id-" + id);
		
	    User user = userOptional.get();
	    
	    post.setUser(user);
	    
	    postRepository.save(post);
	    
	    URI location = ServletUriComponentsBuilder.fromCurrentRequest().path("/{id}").buildAndExpand(post.getId())
				.toUri();
		return ResponseEntity.created(location).build();
	    

	}

}
```



## High Level Microservices problems and evolution
   1. Create Rest Service
   2. Intruducing ``` Configuration using @ConfigurationProperties``` [Solution for Problem (Limit-service): For external configuration ]
   3. Intruducing ``` Spring Cloud Config Server``` [Solution for Problem (spring-cloud-config-server) : For centrallized configuration using Git ]
   4. Intruducing ``` Environment``` [Solution for Problem (currency-exchange) : For Sending enrionments details in response ]
   5. Intruducing ``` JPA ``` [Solution for Problem (currency-exchange) : For connecting data from Data Base ]
   6. Intruducing ``` RestTemplate  ``` [Solution for Problem (currency-conversion) : For invoking Currency exchange service from currency conversion service ]
   7. Intruducing ``` Feign  ``` [Solution for Problem (currency-conversion) : For invoking Currency exchange service from currency conversion service using rest template lot of code but using Feign we can use CurrencyExchangeProxy and @FeignClient(name="currency-exchange", url="localhost:8000") ]
   8. Intruducing ``` Eureka {Naming server/Service Registry}  ``` [Solution for Problem (currency-conversion) : For invoking Currency exchange service using dynamic instace removing hardcode URL from  CurrencyExchangeProxy]
   10.Intruducing ``` Eureka client ``` [Solution for Problem (currency-conversion/currency-exchange) : For registering both the services into Eurka server eureka.client.serviceUrl.defaultZone=http://localhost:8761/eureka in currency exchange proxy @FeignClient(name="currency-exchange") removed URL]
   11. Intruducing ```Spring Cloud LoadBalancer (Previous Option Ribbon ) ``` [Solution for Problem (currency-conversion/currency-exchange) : loadbalancing is required for multiple intances for different serivices Spring CLoude is automatically configure when we use Eureka and Fiegn]
   12. Intruducing ``` Spring Cloud API Gateway (Previous Option Zuul) ``` [Solution for Problem (currency-conversion/currency-exchange) : For common feature implementation like  (Cross cutting concerns)Authentication , logging ,Security and Active/Active configuration we required API gateway , Using API gateway from sigle URL we can access all services]
   13. Intruducing ``` Routing using Spring Cloud API Gateway  ``` [Solution for Problem (currency-conversion/currency-exchange) : Using RouteLocator we can create different routes for different urls .route(p -> p.path("/currency-conversion-feign/**").uri("lb://currency-conversion")) ]
   14. Intruducing ``` GlobalFilter using Spring Cloud API Gateway  ``` [Solution for Problem (currency-conversion/currency-exchange) : We can filter all requests here or we cna do authentication ]
   15. Intruducing ``` Resilience4j/ (Previous Netflix Hystrix) {Circuit Breaker Pattern} ``` [Solution for Problem (currency-conversion/currency-exchange) : Circuit Breaker/fallback response  features 
> A. @Retry(name = "sample-api", fallbackMethod = "hardcodedResponse") 
> 
> - resilience4j.retry.instances.sample-api.maxAttempts=5
> - resilience4j.retry.instances.sample-api.waitDuration=1s
> - resilience4j.retry.instances.sample-api.enableExponentialBackoff=true
> 
>B. @CircuitBreaker(name = "default", fallbackMethod = "hardcodedResponse")
> 
> - resilience4j.circuitbreaker.instances.default.failureRateThreshold=90
>
>C. @RateLimiter(name="default")
> 
> - resilience4j.ratelimiter.instances.default.limitForPeriod=2
> - resilience4j.ratelimiter.instances.default.limitRefreshPeriod=10s
>
>D. @Bulkhead(name="default")
> 
> - resilience4j.bulkhead.instances.default.maxConcurrentCalls=10
> - resilience4j.bulkhead.instances.sample-api.maxConcurrentCalls=10
> ]   

 16. Intruducing ```Distributed Tracing Zipikin ``` [Solution for Problem (currency-conversion/currency-exchange) : Debug problem in complex chain, trace request across microservices]
 17. Intruducing ```Launching Zipkin container ``` [Solution for Problem (currency-conversion/currency-exchange) : Launching Zipkin contianer using Docker]
     ``` docker run -p 9411:9411 openzipkin/zipkin:2.23 ```
 18. Intruducing ```Connecting Microservice to Zipkin/sleuth/spring rabbit mq ``` [Solution for Problem (currency-conversion/currency-exchange) : Connecting to microservices using Zipkin/sleuth.spring rabit mq]
> spring.sleuth.sampler.probability=1.0  all request tracing we can set percentage like 0.5%
 20. Intruducing ```Creating Container Image for Microservices ``` [Solution for Problem (currency-conversion/currency-exchange) : Deploying and running all services  ]
 21. Intruducing ```Docker Compose Launching Container Image for all Microservices ``` [Solution for Problem (currency-conversion/currency-exchange) : Deploying and running all services one by one is not good idea that's will use docker compose ]
 22.  Running all the images using docker compose
   
   
### Docker Conpose
```
version: "3.7"  # optional since v1.27.0
services:
  currency-exchange:
    image: jbirla/mmv2-currency-exchange-service:0.0.1-SNAPSHOT
    mem_limit: 7000m
    ports:
      - "8000:8000"
    networks:
      - currency-jd-network
    depends_on:
      - naming-server
      - rabbitmq
    environment:
      EUREKA.CLIENT.SERVICEURL.DEFAULTZONE: http://naming-server:8761/eureka
      SPRING.ZIPKIN.BASEURL: http://zipkin-server:9411/
      RABBIT_URI: amqp://guest:guest@rabbitmq:5672
      SPRING_RABBITMQ_HOST: rabbitmq
      SPRING_ZIPKIN_SENDER_TYPE: rabbit

  currency-conversion:
    image: jbirla/mmv2-currency-conversion-service:0.0.1-SNAPSHOT
    mem_limit: 7000m
    ports:
      - "8100:8100"
    networks:
      - currency-jd-network
    depends_on:
      - naming-server
      - rabbitmq
    environment:
      EUREKA.CLIENT.SERVICEURL.DEFAULTZONE: http://naming-server:8761/eureka
      SPRING.ZIPKIN.BASEURL: http://zipkin-server:9411/
      RABBIT_URI: amqp://guest:guest@rabbitmq:5672
      SPRING_RABBITMQ_HOST: rabbitmq
      SPRING_ZIPKIN_SENDER_TYPE: rabbit
      
      
  api-gateway:
    image: jbirla/mmv2-api-gateway:0.0.1-SNAPSHOT
    mem_limit: 7000m
    ports:
      - "8765:8765"
    networks:
      - currency-jd-network
    depends_on:
      - naming-server
      - rabbitmq
    environment:
      EUREKA.CLIENT.SERVICEURL.DEFAULTZONE: http://naming-server:8761/eureka
      SPRING.ZIPKIN.BASEURL: http://zipkin-server:9411/
      RABBIT_URI: amqp://guest:guest@rabbitmq:5672
      SPRING_RABBITMQ_HOST: rabbitmq
      SPRING_ZIPKIN_SENDER_TYPE: rabbit
      

  naming-server:
    image: jbirla/mmv2-naming-server:0.0.1-SNAPSHOT
    mem_limit: 7000m
    ports:
      - "8761:8761"
    networks:
      - currency-jd-network
      
  zipkin-server:
    image: openzipkin/zipkin:2.23
    mem_limit: 300m
    ports:
      - "9411:9411"
    networks:
      - currency-jd-network
    environment:
      RABBIT_URI: amqp://guest:guest@rabbitmq:5672
    depends_on:
      - rabbitmq
    restart: always #Restart if there is a problem starting up    
    
  rabbitmq:
    image: rabbitmq:3.8.12-management
    mem_limit: 300m
    ports:
      - "5672:5672"
      - "15672:15672"
    networks:
      - currency-jd-network
    restart: always #Restart if there is a problem starting up



# Networks to be created to facilitate communication between containers
networks:
  currency-jd-network:
  ```
  
### Docker commands

```
docker network create currency-network
docker run -p 8000:8000 --network=currency-network --name=currency-exchange-service in28min/currency-exchange-service:0.0.1-SNAPSHOT
docker run -p 8100:8100 --network=currency-network --name=currency-conversion-service --env CURRENCY_EXCHANGE_URI=http://currency-exchange-service:8000 -d in28min/currency-conversion-service:0.0.1-SNAPSHOT

mvn docker:build
docker run -p 8080:8080 webservices/01-hello-world-rest-api

docker-compose up
```

## High Level Microservices problems and evolution with Kubernetes
 1. Intruducing ``` Docker ``` [Solution for Problem:Standaridized application packagingm languge netural cloud netrual , 1000 microservice ]
 2. Intruducing ``` Kubernetes ``` [Solution for Problem: Container Orchestration manages 1000 instacne of 1000 microservice, Auto scaling, service Discovery , Load balancing , self healing, zero downtime deployments, cloud netural]
 3. Intruducing ``` Deployment.yaml ``` [Solution for Problem: Deploy using the depoyment.yaml]
 4. Intruducing ``` minReadySeconds ``` [Solution for Problem: minReadySeconds is an optional field that specifies the minimum number of seconds for which a newly created Pod should be ready without any of its containers crashing, for it to be considered available]
 5. Intruducing ``` Kompose ``` [Solution for Problem:Kompose is a conversion tool for Docker Compose to container orchestrators such as Kubernetes (or OpenShift). ]
 6. Intruducing ``` Config Maps ``` [Solution for Problem:A ConfigMap is an API object used to store non-confidential data in key-value pairs. Pods can consume ConfigMaps as environment variables, command-line arguments, or as configuration files in a volume. ]
 7. Intruducing ``` Secret ``` [Solution for Problem:A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key. Such information might otherwise be put in a Pod specification or in a container image]
 8. Intruducing ``` Kubernetes with Microservice ``` [Solution for Problem:No need to spring cloud config server and Eureka becasue Kuberenetes provide cofig map for configuration and it provides service discovery for free]
 9. Intruducing ``` Liveness and Readiness ``` [Solution for Problem:If readiness probe is not successful no traffic is sent and if liveness probe is not succesful the pod is restarted]
 10. 
 
### Kubernetes
### Deployement.yaml
```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: currency-conversion
  name: currency-conversion
  namespace: default
spec:
  replicas: 2
  minReadySeconds: 45
  selector:
    matchLabels:
      app: currency-conversion
  strategy:
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: currency-conversion
    spec:
      containers:
      - image: jbirla/k8s-currency-conversion:0.0.5-RELEASE #CHANGE
        imagePullPolicy: IfNotPresent
        name: currency-conversion
#        env: //CHANGE
#          - name: CURRENCY_EXCHANGE_URI
#            value: http://currency-exchange
      restartPolicy: Always
      terminationGracePeriodSeconds: 30
---
apiVersion: v1
kind: Service
metadata:
  labels: #PODS
    app: currency-conversion
  name: currency-conversion
  namespace: default
spec:
  ports:
  - # nodePort: 30701 
    port: 8100 
    protocol: TCP
    targetPort: 8100 
  selector:
    app: currency-conversion
  sessionAffinity: None 
  type: NodePort
  ```
### Kubernetes Commands
```
kubectl create deployment hello-world-rest-api --image=in28min/hello-world-rest-api:0.0.1.RELEASE
kubectl expose deployment hello-world-rest-api --type=LoadBalancer --port=8080
kubectl scale deployment hello-world-rest-api --replicas=3
kubectl delete pod hello-world-rest-api-58ff5dd898-62l9d
kubectl autoscale deployment hello-world-rest-api --max=10 --cpu-percent=70

kubectl get events
kubectl get pods
kubectl get replicaset
kubectl get deployment
kubectl get service

kubectl apply -f deployment.yaml

kubectl delete all -l app=hello-world-rest-api

kubectl apply -f mysql-database-data-volume-persistentvolumeclaim.yaml,mysql-deployment.yaml,mysql-service.yaml

eksctl create cluster --name in28minutes-cluster --nodegroup-name in28minutes-cluster-node-group  --node-type t2.medium --nodes 3 --nodes-min 3 --nodes-max 7 --managed --asg-access

```
