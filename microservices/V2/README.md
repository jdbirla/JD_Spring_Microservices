# Spring Microservices V2

## What You Will Learn during this Step 01:
- Setting up Limits Microservice

On Spring Initializr, choose:
- Group Id: com.in28minutes.microservices
- Artifact Id: limits-service
- Dependencies
	- Web
	- DevTools
	- Actuator
	- Config Client

```properties
spring.config.import=optional:configserver:http://localhost:8888
```
---
## What You Will Learn during this Step 02:

- Creating a hard coded limits service

* /limits-service/src/main/java/com/jd/microservices/limitsservice/controller/LimitsController.java

```java
package com.jd.microservices.limitsservice.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import com.jd.microservices.limitsservice.bean.Limits;

@RestController
public class LimitsController {
	
	@GetMapping("/limits")
	public Limits retrieveLimits() {
		return new Limits(1,1000);
	}

}

```
* com.jd.microservices.limitsservice.bean.Limits
```java
package com.jd.microservices.limitsservice.bean;

public class Limits {
	private int minimum;
	private int maximum;

	public Limits() {
		super();
	}

	public Limits(int minimum, int maximum) {
		super();
		this.minimum = minimum;
		this.maximum = maximum;
	}

	public int getMinimum() {
		return minimum;
	}

	public void setMinimum(int minimum) {
		this.minimum = minimum;
	}

	public int getMaximum() {
		return maximum;
	}

	public void setMaximum(int maximum) {
		this.maximum = maximum;
	}

}
```
* Output

![Browser](Images/Screenshot_01.png)

---
## What You Will Learn during this Step 03:

- Enhance limits service to pick up configuration from application properties

* com.jd.microservices.limitsservice.configuration.Configuration

```java
package com.jd.microservices.limitsservice.configuration;

import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;

@Component
@ConfigurationProperties("limits-service")
public class Configuration {
	private int minimum;
	private int maximum;

	public int getMinimum() {
		return minimum;
	}

	public void setMinimum(int minimum) {
		this.minimum = minimum;
	}

	public int getMaximum() {
		return maximum;
	}

	public void setMaximum(int maximum) {
		this.maximum = maximum;
	}

}
```

* application.properties

```java
spring.config.import=optional:configserver:http://localhost:8888
limits-service.minimum=2
limits-service.maximum=998
```

* com.jd.microservices.limitsservice.controller.LimitsController

```java
@Autowired
	private Configuration configuration;
	
	@GetMapping("/limits")
	public Limits retrieveLimits() {
		//return new Limits(1,1000);
		return new Limits(configuration.getMinimum(),configuration.getMaximum());
	}

```

* Output

![Browser](Images/Screenshot_02.png)

---
## What You Will Learn during this Step 04:

- Setting up Spring Cloud Config Server

* Design
![Browser](Images/Screenshot_03.png)

On Spring Initializr, choose:
- Group Id: com.jd.microservices
- Artifact Id: spring-cloud-config-server
- Dependencies
	- DevTools
	- Config Server

* application.properties

```java
spring.application.name=spring-cloud-config-server
server.port=8888
```
---
## What You Will Learn during this Step 05:

- Installing Git and Creating Local Git Repository

* git-localconfig-repo add properties file limit-service properties

```properties
limits-service.minimum=4
limits-service.maximum=996
```
---
## What You Will Learn during this Step 06:

- Connect Spring Cloud Config Server to Local Git Repository

* /spring-cloud-config-server/src/main/java/com/jd/microservices/springcloudconfigserver/SpringCloudConfigServerApplication.java

```java
@EnableConfigServer
@SpringBootApplication
public class SpringCloudConfigServerApplication {

	public static void main(String[] args) {
		SpringApplication.run(SpringCloudConfigServerApplication.class, args);
	}

}
```

* /spring-cloud-config-server/src/main/resources/application.properties

```properties
spring.application.name=spring-cloud-config-server
server.port=8888
spring.cloud.config.server.git.uri=file:///C:/JD_Spring_Microservices/microservices/V2/local-git-localconfig-repo
```
* Local git repository
![Browser](Images/Screenshot_04.png)
![Browser](Images/Screenshot_05.png)

* Output
![Browser](Images/Screenshot_06.png)

---
## What You Will Learn during this Step 07:
- Connect Limits Service to Spring Cloud Config Server
- spring.application.name=limit-service  name should be exact same as local-git-localconfig-repo/limit-service properties file
-  Config server has higher priority then local 

URLS
- http://localhost:8888/limits-service/default
- http://localhost:8080/limits

* /limits-service/src/main/resources/application.properties
```properties
spring.application.name=limit-service
spring.config.import=optional:configserver:http://localhost:8888
limits-service.minimum=2
limits-service.maximum=998
```

* Local git repository config server
![Browser](Images/Screenshot_04.png)
![Browser](Images/Screenshot_05.png)

* Limit service now connected to config server and getting values from local git
![Browser](Images/Screenshot_07.png)

---
## What You Will Learn during this Step 08:
- Configuring Profiles for Limits Service

- http://localhost:8888/limits-service/default
- http://localhost:8888/limits-service/qa
- http://localhost:8888/limits-service/dev

* /limits-service/src/main/resources/application.properties added profiles as dev

```properties
spring.application.name=limit-service
spring.config.import=optional:configserver:http://localhost:8888

spring.profiles.active=dev

limits-service.minimum=2
limits-service.maximum=998

```

#### JD_Spring_Microservices/microservices/V2/local-git-localconfig-repo/limit-service.properties existing
```properties
limits-service.minimum=4
limits-service.maximum=990
```


#### JD_Spring_Microservices/microservices/V2/local-git-localconfig-repo/limit-service-dev.properties New
```properties
limits-service.minimum=5
limits-service.maximum=995
```

#### JD_Spring_Microservices/microservices/V2/local-git-localconfig-repo/limit-service-qa.properties New
```properties
limits-service.minimum=6
limits-service.maximum=994

```


#### JD_Spring_Microservices/microservices/V2/local-git-localconfig-repo/microservice-x-dev.properties New
```properties
limits-service.minimum=4
limits-service.maximum=996
```


#### JD_Spring_Microservices/microservices/V2/local-git-localconfig-repo/microservice-x.properties New
```properties
limits-service.minimum=4
limits-service.maximum=996
```


#### JD_Spring_Microservices/microservices/V2/local-git-localconfig-repo/microservice-y.properties New
```properties
limits-service.minimum=4
limits-service.maximum=996
```

* Config server default profiles
![Browser](Images/Screenshot_08.png)

* Config server dev profiles
![Browser](Images/Screenshot_09.png)

* Config server qa profiles
![Browser](Images/Screenshot_09.png)

* limit service connected with Config server with dev profiles active
![Browser](Images/Screenshot_10.png)

---
