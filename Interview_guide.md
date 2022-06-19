# spring-microservices interview guide
- https://github.com/jdbirla/JD_Spring_Microservices/tree/master/Documents
- [Restful] https://github.com/jdbirla/JD_Spring_Microservices/blob/master/restful-web-services/SM_Restful_Web_Services_Detail.md
- [Microservice] https://github.com/jdbirla/JD_Spring_Microservices/blob/master/microservices/V2/README.md


## High Level Microservices problems and evolution
   1. Create Rest Service
   2. Intruducing ``` Configuration using @ConfigurationProperties``` [Solution for Problem (Limit-service): For external configuration ]
   3. Intruducing ``` Spring Cloud Config Server``` [Solution for Problem (spring-cloud-config-server) : For centrallized configuration using Git ]
   4. Intruducing ``` Spring Cloud Config Server``` [Solution for Problem (spring-cloud-config-server) : For centrallized configuration using Git ]
   5. Intruducing ``` Environment``` [Solution for Problem (currency-exchange) : For Sending enrionments details in response ]
   6. Intruducing ``` JPA ``` [Solution for Problem (currency-exchange) : For connecting data from Data Base ]
   7. Intruducing ``` RestTemplate  ``` [Solution for Problem (currency-conversion) : For invoking Currency exchange service from currency conversion service ]
   8. Intruducing ``` Feign  ``` [Solution for Problem (currency-conversion) : For invoking Currency exchange service from currency conversion service using rest template lot of code but using Feign we can use CurrencyExchangeProxy and @FeignClient(name="currency-exchange", url="localhost:8000") ]
   9. Intruducing ``` Eureka {Naming server/Service Registry}  ``` [Solution for Problem (currency-conversion) : For invoking Currency exchange service using dynamic instace removing hardcode URL from  CurrencyExchangeProxy]
   10.Intruducing ``` Eureka client ``` [Solution for Problem (currency-conversion/currency-exchange) : For registering both the services into Eurka server eureka.client.serviceUrl.defaultZone=http://localhost:8761/eureka in currency exchange proxy @FeignClient(name="currency-exchange") removed URL]
   11. Intruducing ```Spring Cloud LoadBalancer (Another Option Ribbon ) ``` [Solution for Problem (currency-conversion/currency-exchange) : loadbalancing is required for multiple intances for different serivices Spring CLoude is automatically configure when we use Eureka and Fiegn]
   12. 
   
   
