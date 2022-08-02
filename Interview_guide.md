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
```
Response Status
200 - SUCCESS
404 - RESOURCE NOT FOUND
400 - BAD REQUEST
201 - CREATED
401 - UNAUTHORIZED
500 - SERVER ERROR
```
- Rest Api conroller
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

- - Rest Api conroller using JPA


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
- REST Unit test

```java
package com.jd.springboot.controller;

import static org.junit.Assert.assertEquals;

import java.util.Arrays;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.Mockito;
import org.skyscreamer.jsonassert.JSONAssert;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.boot.test.mock.mockito.MockBean;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpStatus;
import org.springframework.http.MediaType;
import org.springframework.mock.web.MockHttpServletResponse;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.MvcResult;
import org.springframework.test.web.servlet.RequestBuilder;
import org.springframework.test.web.servlet.request.MockMvcRequestBuilders;

import com.jd.springboot.model.Question;
import com.jd.springboot.service.SurveyService;

@RunWith(SpringRunner.class)
@WebMvcTest(value = SurveyController.class)
public class SurveyControllerTest {

	@Autowired
	private MockMvc mockMvc;

	// Mock @Autowired
	@MockBean
	private SurveyService surveyService;
	
	@Test
	public void retrieveDetailsForQuestion() throws Exception {
		Question mockQuestion = new Question("Question1",
				"Largest Country in the World", "Russia", Arrays.asList(
						"India", "Russia", "United States", "China"));

		Mockito.when(
				surveyService.retrieveQuestion(Mockito.anyString(), Mockito
						.anyString())).thenReturn(mockQuestion);

		RequestBuilder requestBuilder = MockMvcRequestBuilders.get(
				"/surveys/Survey1/questions/Question1").accept(
				MediaType.APPLICATION_JSON);

		MvcResult result = mockMvc.perform(requestBuilder).andReturn();

		//String expected = "{id:Question1,description:Largest Country in the World,correctAnswer:Russia}";
        String expected = "{\"id\":\"Question1\",\"description\":\"Largest Country in the World\",\"correctAnswer\":\"Russia\",\"options\":[\"India\",\"Russia\",\"United States\",\"China\"]}";

		
		  JSONAssert.assertEquals(expected, result.getResponse() .getContentAsString(),
		  false);
		 

		// Assert
	}
	
	@Test
    public void createSurveyQuestion() throws Exception {
    		Question mockQuestion = new Question("1", "Smallest Number", "1",
				Arrays.asList("1", "2", "3", "4"));

		String questionJson = "{\"description\":\"Smallest Number\",\"correctAnswer\":\"1\",\"options\":[\"1\",\"2\",\"3\",\"4\"]}";
		//surveyService.addQuestion to respond back with mockQuestion
		Mockito.when(
				surveyService.addQuestion(Mockito.anyString(), Mockito
						.any(Question.class))).thenReturn(mockQuestion);

		//Send question as body to /surveys/Survey1/questions
		RequestBuilder requestBuilder = MockMvcRequestBuilders.post(
				"/surveys/Survey1/questions")
				.accept(MediaType.APPLICATION_JSON).content(questionJson)
				.contentType(MediaType.APPLICATION_JSON);

		MvcResult result = mockMvc.perform(requestBuilder).andReturn();

		MockHttpServletResponse response = result.getResponse();

		assertEquals(HttpStatus.CREATED.value(), response.getStatus());

		assertEquals("http://localhost/surveys/Survey1/questions/1", response
				.getHeader(HttpHeaders.LOCATION));
    
    }
}

```
- Rest API IT

```java
package com.jd.springboot.controller;

import static org.junit.jupiter.api.Assertions.assertTrue;

import java.nio.charset.Charset;
import java.util.Arrays;
import java.util.List;

import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.skyscreamer.jsonassert.JSONAssert;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.web.client.TestRestTemplate;
import org.springframework.boot.web.server.LocalServerPort;
import org.springframework.core.ParameterizedTypeReference;
import org.springframework.http.HttpEntity;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpMethod;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.security.crypto.codec.Base64;
import org.springframework.test.context.junit4.SpringRunner;

import com.jd.springboot.Application;
import com.jd.springboot.model.Question;

@RunWith(SpringRunner.class)
@SpringBootTest(classes = Application.class, webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class SurveyControllerIT {

	@LocalServerPort
	private int port;

	private TestRestTemplate template = new TestRestTemplate();

    HttpHeaders headers = new HttpHeaders();

    @Before
    public void setupJSONAcceptType() {
    	System.out.println("Before-JD");
    	headers.add("Authorization", createHttpAuthenticationHeaderValue(
				"user1", "secret1"));
        headers.setAccept(Arrays.asList(MediaType.APPLICATION_JSON));
    }

    private String createUrl(String uri) {
        return "http://localhost:" + port + uri;
    }

    private String createHttpAuthenticationHeaderValue(String userId,
			String password) {

		String auth = userId + ":" + password;

		byte[] encodedAuth = Base64.encode(auth.getBytes(Charset
				.forName("US-ASCII")));

		String headerValue = "Basic " + new String(encodedAuth);

		return headerValue;
	}
    
    @Test
    public void testSurveyQuestion() throws Exception {

        String expected = "{\"id\":\"Question1\",\"description\":\"Largest Country in the World\",\"correctAnswer\":\"Russia\",\"options\":[\"India\",\"Russia\",\"United States\",\"China\"]}";
        				 //{id:Question1,description:Largest Country in the World,correctAnswer:Russia,options:[India,Russia,United States,China]}
        String retrieveSpecificQueURL = "/surveys/Survey1/questions/Question1";
		ResponseEntity<String> response = template.exchange(
                createUrl(retrieveSpecificQueURL),
                HttpMethod.GET, new HttpEntity<String>("DUMMY_DOESNT_MATTER",
                        headers), String.class);

          System.out.println(" response.getBody() -> "+ response.getBody());
          System.out.println(" expected -> "+ expected);
        JSONAssert.assertEquals(expected, response.getBody(), false);
    }

    @Test
    public void retrieveSurveyQuestions() throws Exception {
        String retreiveAllUqe = "/surveys/Survey1/questions/";
		ResponseEntity<List<Question>> response = template.exchange(
                createUrl(retreiveAllUqe), HttpMethod.GET,
                new HttpEntity<String>("DUMMY_DOESNT_MATTER", headers),
                new ParameterizedTypeReference<List<Question>>() {
                });

        Question sampleQuestion = new Question("Question1",
                "Largest Country in the World", "Russia", Arrays.asList(
                        "India", "Russia", "United States", "China"));

        System.out.println(" response.getBody() -> "+ response.getBody());

        assertTrue(response.getBody().contains(sampleQuestion));
    }
  
  //NEEDS REFACTORING
  	@Test
  	public void addQuestion() {

  		String url = "http://localhost:" + port + "/surveys/Survey1/questions";

  		TestRestTemplate restTemplate = new TestRestTemplate();

  		HttpHeaders headers = new HttpHeaders();

  		headers.setAccept(Arrays.asList(MediaType.APPLICATION_JSON));

  		Question question = new Question("DOESNTMATTER", "QuestionJD", "Russia",
  				Arrays.asList("India", "Russia", "United States", "China"));

  		HttpEntity entity = new HttpEntity<Question>(question, headers);

  		ResponseEntity<String> response = restTemplate.exchange(url,
  				HttpMethod.POST, entity, String.class);

  		String actual = response.getHeaders().get(HttpHeaders.LOCATION).get(0);

        System.out.println(" actual -> "+ actual);

  		assertTrue(actual.contains("/surveys/Survey1/questions/"));

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
   
- Currency Converion controller
```java
package com.jd.microservices.currencyconversionservice;

import java.math.BigDecimal;
import java.util.HashMap;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

@RestController
public class CurrencyConversionController {

	@Autowired
	private CurrencyExchangeProxy currencyExchangeProxy;

	@GetMapping("/currency-conversion/from/{from}/to/{to}/quantity/{quantity}")
	public CurrencyConversion calculateCurrencyConversion(@PathVariable String from, @PathVariable String to,
			@PathVariable BigDecimal quantity) {

		HashMap<String, String> uriVariables = new HashMap<String, String>();
		uriVariables.put("from", from);
		uriVariables.put("to", to);
		ResponseEntity<CurrencyConversion> reponseEntity = new RestTemplate().getForEntity(
				"http://localhost:8000/currency-exchange/from/{from}/to/{to}", CurrencyConversion.class, uriVariables);

		CurrencyConversion currencyConversion = reponseEntity.getBody();
		return new CurrencyConversion(currencyConversion.getId(), from, to, quantity,
				currencyConversion.getConversionMultiple(),
				quantity.multiply(currencyConversion.getConversionMultiple()), currencyConversion.getEnvironment()+ " " + "REST");
	}

	@GetMapping("/currency-conversion-feign/from/{from}/to/{to}/quantity/{quantity}")
	public CurrencyConversion calculateCurrencyConversionFeign(@PathVariable String from, @PathVariable String to,
			@PathVariable BigDecimal quantity) {

		CurrencyConversion currencyConversion = currencyExchangeProxy.retrieveExchangeValue(from, to);
		return new CurrencyConversion(currencyConversion.getId(), from, to, quantity,
				currencyConversion.getConversionMultiple(),
				quantity.multiply(currencyConversion.getConversionMultiple()), currencyConversion.getEnvironment() + " " + "feign");
	}

}
```
   
# Docker 

## Docker With Java Spring Boot Hello World Rest APIs
### Building an Image Manually
 1. Intruducing ``` Docker ``` [Solution for Problem: Create docker Image and run]
 ```
 1. Build a Jar - /target/hello-world-rest-api.jar
2. Setup the Prerequisites for Running the JAR - openjdk:8-jdk-alpine
3. Copy the jar
4. Run the jar

### Docker Commands - Creating Image Manually

- docker run -dit openjdk:8-jdk-alpine
- docker container exec naughty_knuth ls /tmp
- docker container cp target/hello-world-rest-api.jar naughty_knuth:/tmp
- docker container exec naughty_knuth ls /tmp
- docker container commit naughty_knuth jbirla/hello-world-rest-api:manual1
- docker run jbirla/hello-world-rest-api:manual1
- docker container ls
- docker container commit --change='CMD ["java","-jar","/tmp/hello-world-rest-api.jar"]' naughty_knuth jbirla/hello-world-rest-api:manual2
- docker run -p 8080:8080 jbirla/hello-world-rest-api:manual2
 
 ```
 ### Building an Image Using docker file 
  2. Intruducing ``` Dockerfile ``` [Solution for Problem: In previoud step we have to do lot maual work]
  ```
  FROM openjdk:8-jdk-alpine
EXPOSE 8080
ADD target/hello-world-rest-api.jar hello-world-rest-api.jar
ENTRYPOINT ["sh", "-c", "java -jar /hello-world-rest-api.jar"]
  ```
 ### Using Docker Spotify Plugin to create docker images
  3. Intruducing ``` Spotify Maven Plugin ``` [Solution for Problem: It integrates with maven and using maven clean build]

 ### Generic Reusable Dockerfile
  4. Intruducing ``` Generic Docker file ``` [Solution for Problem: Resuable docker file]
  ```
  FROM openjdk:8-jdk-alpine
EXPOSE 8080
ADD target/*.jar app.jar
ENTRYPOINT ["sh", "-c", "java -jar /app.jar"]
```
 ### Using JIB plugin to create docker images
  5. Intruducing ``` JIB Plugin ``` [Solution for Problem: Jib builds containers without using a Dockerfile or requiring a Docker installation. You can use Jib in the Jib plugins for Maven ]

 ### Using Fabric8 docker maven plugin to create docker images
  6. Intruducing ``` Fabric8 docker maven plugin ``` [Solution for Problem:  This is a Maven plugin for managing Docker images and containers]

----
## Docker With Java Spring Boot Todo WEB APP
  1. Intruducing ``` Create docker image for Web ``` [Solution for Problem:  Docker image for Spring boot web app]
```
FROM tomcat:8.0.51-jre8-alpine
EXPOSE 8080
RUN rm -rf /usr/local/tomcat/webapps/*
COPY target/*.war /usr/local/tomcat/webapps/ROOT.war
CMD ["catalina.sh","run"]
```
## Docker With Java Spring Boot Todo WEB APP using MySQL

### MySQL + Web App

  1. Intruducing ``` Create docker image for Web using MySQL ``` [Solution for Problem:  Docker image for Spring boot web app using MySQL]

#### Launching MySQL and Web App using Link in bridge network

2. Intruducing ``` Launching MySQL using Docker ``` [Solution for Problem:  Docker image running for MYSQL]
  
```
docker run --detach --env MYSQL_ROOT_PASSWORD=dummypassword --env MYSQL_USER=todos-user --env MYSQL_PASSWORD=dummytodos --env MYSQL_DATABASE=todos --name mysql --     publish 3306:3306 mysql:5.7
```

 3. Intruducing ``` Launching Web App using link with MySQL (deprecated) ``` [Solution for Problem:  Two cotainer can't communicate becasue they use default bridge network which used default setting we need to link both]
 
 ```
 docker container run -p 8080:8080 --link=mysql -e RDS_HOSTNAME=mysql  in28min/todo-web-application-mysql:0.0.1-SNAPSHOT
 ```
 #### Launching MySQL and Web App using Custom Network
 

 4. Intruducing ``` Launching Web App using MYSQL using custom network ``` [Solution for Problem: We can't use host network in windows and mac as docker alway run in these OS as docker desktop while network host can run in unix in below link not required]
 * MySQL
```
docker run --detach --env MYSQL_ROOT_PASSWORD=dummypassword --env MYSQL_USER=todos-user --env MYSQL_PASSWORD=dummytodos --env MYSQL_DATABASE=todos --name mysql --publish 3306:3306 --network=web-application-mysql-network mysql:5.7
```
* Web App 
```
docker container run -p 8080:8080 --network=web-application-mysql-network -e RDS_HOSTNAME=mysql  in28min/todo-web-application-mysql:0.0.1-SNAPSHOT
```

### MySQL Using a Volume for persist data

 4. Intruducing ``` Launching MYSQL using volume ``` [Solution for Problem: When MySQL container will stop and re launch all data will be deleted]
```
docker run --detach --env MYSQL_ROOT_PASSWORD=dummypassword --env MYSQL_USER=todos-user --env MYSQL_PASSWORD=dummytodos --env MYSQL_DATABASE=todos --name mysql --publish 3306:3306 --network=web-application-mysql-network --volume mysql-database-volume:/var/lib/mysql  mysql:5.7
```

## Docker With Java Spring Boot React Full stack Application



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
---

