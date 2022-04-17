# Spring Micro services Restful Webservices

## What You Will Learn during this Step 01:
- Lets create a simple restful web application using Spring Boot
- Lets run the Spring Boot Application
- There is a lot of magic happening in here!

---
## What You Will Learn during this Step 02:
- Step 02 - Understanding the RESTful Services we would create in this course

- Social Media Application

- User -> Posts


- Retrieve all Users    - GET /users
- Create a User         - POST /users
- Retrieve one User     - GET /users/{id} -> /users/1
- Delete a User         - DELETE /users/{id} -> /users/1


- Retrieve all posts for a User  - GET /users/{id}/posts
- Create a posts for a User      - POST /users/{id}/posts
- Retrieve details of a post     - GET /users/{id}/posts/{post_id}

---
## What You Will Learn during this Step 03:
- Creating a Hello World Service

---
## What You Will Learn during this Step 04:
- Enhancing the Hello World Service to return a Bean

---
## What You Will Learn during this Step 05:
- Quick Review of Spring Boot Auto Configuration and Dispatcher Servlet - What's happening in the background?

![Browser](Images/Screenshot_01.png)

---
## What You Will Learn during this Step 06:

- Enhancing the Hello World Service with a Path Variable

---
## What You Will Learn during this Step 07 & 08 & 09 & 10:

- Step 07 - Creating User Bean and User Service
- Step 08 - Implementing GET Methods for User Resource
- Step 09 - Implementing POST Method to create User Resource
- Step 10 - Enhancing POST Method to return correct HTTP Status Code and Location URI

![Browser](Images/Screenshot_02.png)
![Browser](Images/Screenshot_03.png)
![Browser](Images/Screenshot_04.png)
![Browser](Images/Screenshot_05.png)
![Browser](Images/Screenshot_06.png)

---
## What You Will Learn during this Step 11 & 12 & 13:

- Step 11 - Implementing Exception Handling - 404 Resource Not Found
- Step 12 - Implementing Generic Exception Handling for all Resources
- Step 13 - Exercise : User Post Resource and Exception Handling

---
## What You Will Learn during this Step 14:
- Step 14 - Implementing DELETE Method to delete a User Resource

---
## What You Will Learn during this Step 15:
- Step 15 - Implementing Validations for RESTful Services

* pom.xml ad dependency of spring-boot-starter-validation
```pom.xml
</dependency>
			<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-validation</artifactId>
		</dependency>
```

* output
![Browser](Images/Screenshot_07.png)

---
## What You Will Learn during this Step 16:

- Step 16 - Implementing HATEOAS for RESTful Services

* pom.xml add dependency 

```pom.xml
	<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-hateoas</artifactId>
		</dependency>
```
* com.jd.rest.webservices.restfulwebservices.user.UserResource 
```java	
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
```

* output
![Browser](Images/Screenshot_08.png)

---
## What You Will Learn during this Step 17 and 18:

- Step 17 - Overview of Advanced RESTful Service Features
- Step 18 - Internationalization for RESTful Services

* com.jd.rest.webservices.restfulwebservices.HelloWorldController
```java
@GetMapping(path = "/hello-world-internationalized")
	public String helloWorldInternationalized(
		//	@RequestHeader(name = "Accept-Language" ,required = false) Locale locale
			) {
		//return "Hello World";
		return messageSource.getMessage("good.morning.message", null, LocaleContextHolder.getLocale());
		
	}
```
* messages.properties
```properties
good.morning.message = Good Morning
```

* messages_fr.properties
```properties
good.morning.message = Bonjour
```

* messages_nl.properties
```properties
good.morning.message = Goede Morgen
```

* output
![Browser](Images/Screenshot_09.png)

![Browser](Images/Screenshot_10.png)

---
