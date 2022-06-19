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
 18. Intruducing ```Launching Zipkin container ``` [Solution for Problem (currency-conversion/currency-exchange) : Launching Zipkin contianer using Docker]
 19. 
   


