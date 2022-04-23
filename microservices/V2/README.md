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




